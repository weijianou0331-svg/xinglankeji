# OPCTEAM.md

This file provides guidance to opcteam (bytecoder.ai/code) when working with code in this repository.

## 仓库性质

星澜智能科技（五十人软件定制公司，客户以物流、制造行业为主）AI 工程化咨询项目的文档工作区。**是文档仓库**：以 git 管理（remote：git@gitee.com:weijianou/starly-waves.git），无构建/测试系统，全部内容为中文文档。面向用户的产出与交流一律使用中文。

目录名与文件名含中文和空格，shell 命令中引用路径必须加引号。

## 文档地图（输入 → 产出）

| 文件 | 角色 |
|---|---|
| `共享/需求/星澜智能科技.md` | 客户需求基线：背景/痛点/目标/边界/验收条款。一切产出的验收依据，术语与硬数字以此为准 |
| `共享/方法论/BYTEBROAD_AI工程概论 03.31.17.md` | 方法论一：FDE、五阶段流水线、多 Agent 编排、质量 Gate、SOP 面向 Agent、Token 计量 |
| `共享/方法论/BYTEBROAD_企业AI Native组织的工程化(1).md` | 方法论二：组织治理——先立后破、效果验收（随机抽人驱动 Agent 产出一致性）、计量看板、四模块 |
| `共享/解决方案/星澜智能科技_SubAgents_Coding解决方案.html` | 当前唯一产出物：Coding 解决方案（由上述需求 + 两本方法论生成） |

新文档按类型放入 `共享/需求`、`共享/方法论`、`共享/解决方案`，不要在根目录新增散落文件。

需求文档的「验收」条款与方案 `§08 验收对照` 逐条对应：改动需求中的验收数字或新增验收项时，必须同步更新方案 §08（反之亦然）。

## 产出物（解决方案 HTML）的约定

单文件、自包含：内联 CSS + 少量原生 JS，无外部依赖，本地浏览器直接打开即可浏览。

固定章节结构，侧栏 `<a href="#sX">` 与 `<section id="sX">` 必须一一对应：

- `s0` 方案摘要 · `s1` 现状诊断 · `s2` 解决方案总览 · `s3` 工程产出物目录规范（双仓架构）· `s4` 三域协作结构 · `s5` 全生命周期流水线 · `s6` 质量 Gate 细则 · `s7` 实施路线图 · `s8` 验收对照 · `s9` 附录（SOP 2.0 模板 + 术语表）

新增/删除章节时同步更新侧栏 href 与 section id，并顺延其余编号（附录为 `s9`，无编号数字）。

术语与硬数字必须与需求文档一致，不得自造或漂移：

- 组织：FDE（人只站两端：定义标准、验收结果）、SubAgents 三域十席——项目管理域（PMAgent 编排中枢 / ProductAgent 需求收敛 / ArchitectAgent 技术蓝图 / DistillAgent 复盘沉淀）、Coding 域（Backend / Frontend / Api）、品控域（QAAgent 质量 Gate / ReviewAgent 独立评审 / DeployAgent 部署交付）
- 流程：五阶段流水线（需求→设计→单点实现→多 Agent 编排→测试部署复盘）、六道质量 Gate（G1 需求基线 … G6 交付验收）、SOP 2.0 四要素（触发场景/调用步骤/输入规范/产出验收，§04 已为三域 10 席逐席编写）、MoSCoW 冻结需求基线、契约先行、先串后并、效果验收、四维复盘（提效/质量/成本/经验）、有罪推定（质量 Gate 对所有产出默认不合格，证据不足即退回，非证据不足即放行）、PDCA 四步循环（管理域 Plan→Do→Check→Act 严格走完，不得跳步、不得带病交付，每步量化反馈：进度/Gate 通过率/Token 消耗/Bug 数/覆盖率）
- 硬数字：各质量 Gate 通过率 >90%；测试四层（单元/集成/E2E/性能）业务覆盖率 ≥95%；新环境部署+冒烟 ≤30 分钟；并行编排耗时 ≤ 串行 50%；AI 覆盖率 20% → 70% → 90%+；人均项目数 N人/1项目 → 1人/1项目 → 1人/3项目+
- 目录规范（§03）：双仓物理分离——组织级资产库 ai-assets（规约/SOP/模板/组件/Agent 库/Skill，独立 Git、PR 审批、只读消费）× 项目级工作区 `<project>`（每项目一仓、Gate 通过才合主干、交付归档）；目录名即语义（禁 misc/tmp 等无语义目录）、每层 README 索引、根目录 OPCTEAM.md 为 Agent 进入即读的上下文锚点（身份/边界/目录地图/SOP 指针/安全红线）
- 安全红线（安全铁律）：凭据（账号/密码/API Key）只在本地配置和使用，零穿透大模型服务；Agent「使用」凭据但从不「看见」——凭据不入 prompt/上下文/日志/复盘记录；ReviewAgent 执行 G-sec 凭据泄露扫描，发现明文即拦截并告警

## 版本与恢复（无 git 的替代机制）

`.opcteam/` 是 opcteam harness 的内部目录（sessions、history.jsonl、file-history、backups、plans、memory 等），不是项目内容——不要将其作为交付物引用、修改或删除。跨会话的持久记忆由 harness 自动管理，无需人工维护。

本仓库已启用 git（remote：git@gitee.com:weijianou/starly-waves.git），`.opcteam/` 不入库。回退优先用 git；历史版本另由 `.opcteam/file-history/<session-id>/<文件名hash>@vN` 保存快照——先用 `wc -l` 与关键标记核对目标快照（确认是预期版本，而非仅靠文件名判断），再 `cp` 覆盖当前文件，最后重跑「校验命令」三条。

## 编辑 HTML 的操作要点

- 编辑前必须先 Read 该文件；文件被外部（如脚本/命令）改动后，需重新 Read 才能 Edit。
- 大文件分段写入：单次 Write 内容过大会失败。方法：先写骨架并留占位注释（`<!--#PARTn#-->`），再用 Edit 逐段替换占位符；`</div></main></body></html>` 等收尾标签只保留一份，放在最后一次替换之后，避免尾部出现重复闭合。
- 本仓库无自动化测试；每次改动 HTML 后用下方校验命令自查。

## 校验命令（本仓库的 build/lint/test 等价物）

标签平衡与未闭合标签（HTMLParser 会自动吞掉 script/style 内容，比正则计数可靠）：

```bash
python3 - <<'PY'
import html.parser
p='共享/解决方案/星澜智能科技_SubAgents_Coding解决方案.html'
src=open(p,encoding='utf-8').read()
V={'meta','br','hr','img','input','link'}
class C(html.parser.HTMLParser):
    def __init__(self): super().__init__(); self.st=[]; self.err=[]
    def handle_starttag(self,t,a):
        if t not in V: self.st.append(t)
    def handle_endtag(self,t):
        if t in V: return
        if not self.st or self.st[-1]!=t: self.err.append('bad </%s>'%t); return
        self.st.pop()
c=C(); c.feed(src)
assert not c.err and not c.st, (c.err, c.st)
print('tag balance OK')
PY
```

锚点一致性（section id 与侧栏 href 一一对应，含 `s\d+` 编号顺序）：

```bash
python3 -c "import re; s=open('共享/解决方案/星澜智能科技_SubAgents_Coding解决方案.html',encoding='utf-8').read(); a=sorted(set(re.findall(r'href=\"#(s\d+)\"',s))); i=sorted(set(re.findall(r'<section id=\"(s\d+)\">',s))); assert a==i,(a,i); print('anchors OK:',a)"
```

CSS 花括号平衡：

```bash
python3 -c "import re; s=open('共享/解决方案/星澜智能科技_SubAgents_Coding解决方案.html',encoding='utf-8').read(); c=re.search(r'<style>(.*?)</style>',s,re.S).group(1); assert c.count('{')==c.count('}'); print('css braces OK')"
```
