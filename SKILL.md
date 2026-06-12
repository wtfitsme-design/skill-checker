---
name: skill-checker
version: "1.1.0"
description: "Reviews a SKILL.md file and evaluates whether it is well-constructed, using best practices distilled from real-world skill analysis. Use when the user asks to check, review, or validate a skill they have built or are building. Triggers: '帮我检查这个skill', '我写的skill有没有问题', '我写的skill好不好', 'review my SKILL.md', 'validate my skill', '检查一下我的skill'. Two modes: quick (P0 only, ~30 seconds) or full (P0+P1+P2, comprehensive). NOT for executing skills, NOT for creating new skills from scratch."
allowed-tools: Read
---

# skill-checker

根据对7个真实开源 skill 案例的纵横深挖，提炼出的检查标准，评估一个 skill 的构建质量。

## When to Use
- 用户分享了 SKILL.md 内容，请求检查/review/验证
- 用户问"这个 skill 写得好吗""有没有问题"
- 用户完成 skill 构建，想发布前做最后把关

## When NOT to Use
- 用户想**创建**新 skill（换用专门的创建类 skill）
- 用户想**执行**某个 skill 的功能
- 用户只是问 skill 的概念问题

---

## Workflow

### Step 1 · 获取待检查的 skill

如果用户没有直接提供 SKILL.md 内容，询问：
> "请把你的 SKILL.md 内容粘贴过来，或者告诉我文件路径。"

如果给了路径，用 Read 工具读取。

### Step 2 · 确认检查模式

**默认：完整模式**（P0 + P1 + P2 全面检查，12条）

如果用户明确说"快速检查""只看 P0""时间紧"，则切换到快速模式（只检查 P0 核心3条，约30秒）。否则直接执行完整检查。

### Step 3 · 执行检查

按所选模式，逐条检查并判定：✅ 通过 / ⚠️ 需改进 / ❌ 缺失

### Step 4 · 输出报告

见"输出格式"节。

---

## 检查标准

### P0 · 核心项（快速模式只检查这3条）

**P0-1 · description 质量**
检查 description 是否同时包含：
- 做什么（功能描述）
- 何时用（触发场景/触发词）
- 何时**别**用（边界，防误触发）

判定：
- ✅ 三项齐全，触发词具体（如写了"用户会遇到的具体问题信号"）
- ⚠️ 缺"何时别用"，或触发词过于泛泛（如只写"helps with X"）
- ❌ description 只有功能描述，没有触发场景；或极度模糊

**P0-2 · 职责单一性**
检查这个 skill 是否只干一件能干净归类的事。

判定：
- ✅ 明确聚焦单一职责，有"NOT for…"边界声明
- ⚠️ 职责略宽，但尚可接受；或缺少"Not for"但实际内容单一
- ❌ 试图同时做多件不相关的事（如"规划+写作+发布+分析"全干）

**P0-3 · 失败分支**
检查 skill 是否有失败处理机制：当依赖的工具/API/数据源/外部服务失败时，是否写明了处理方式。

判定：
- ✅ 明确写死失败分支（"失败即停""禁止兜底""失败时报错等用户"）
- ⚠️ 有模糊的错误提示，但没有禁止兜底行为
- ❌ 只写正常路径，完全没有失败处理（最危险：模型会偷偷降级）
- N/A：该 skill 不依赖外部工具/数据（如纯方法论类），此项跳过

---

### P1 · 重要项（完整模式追加）

**P1-1 · 配置/规则的具体性**
检查 skill 内部的配置、规则、约束是否具体可执行（非口号）。

判定：
- ✅ 给了具体标准/维度/正反例（如"核心用户判断依据复购频次/抗周期性/成瘾性"）
- ⚠️ 规则存在但过于抽象（如"保持高质量""专业风格"）
- ❌ 配置/规则是口号级别，模型无法执行

**P1-2 · 产出可判定性**
检查是否有明确的验收标准，让产出的好坏可以被判断。

判定：
- ✅ 有量化评分/等级/验收清单（如"目标10/10""S/A/B/C/D等级""官方源≥30%"）
- ⚠️ 有定性描述的输出要求，但无量化标准
- ❌ 只说"给出建议"，没有任何标准说明什么是好的输出

**P1-3 · 正文长度**
检查 SKILL.md 正文行数是否在合理范围。

判定：
- ✅ <500行（官方红线），或虽较长但有 references/ 分文件承载细节
- ⚠️ 300-500行，存在重复内容或可拆分到 references 的部分（内部建议：超过300行就开始考虑拆分）
- ❌ >500行，且正文有大量重复步骤/例子堆砌（如 radar 的874行）

**P1-4 · 渐进式披露**
检查是否合理使用了 references/ 目录分担细节。

判定：
- ✅ 正文做导航，细节拆进 references/；或明确标注"何时读哪个文件"
- ⚠️ 有 references/ 但正文仍有大量细节可以下沉
- ❌ 所有内容全堆在 SKILL.md，没有分文件
- N/A：skill 本身较简单（<150行），不需要 references/

---

### P2 · 加分项（完整模式追加）

**P2-1 · 样例质量**
检查是否有具体的好/坏输入输出样例。

判定：
- ✅ 有具体样例（好输入+好输出，或正反例对比）
- ⚠️ 有抽象描述但没有具体例子
- ❌ 完全没有样例

**正反例参考**（用于校准判定标准）：

❌ 没有样例的 description（P2-1 = ❌）：
```
description: "Helps you write better code."
```

✅ 有具体样例的 description（P2-1 = ✅）：
```
description: "Reviews Rust code for ownership and borrow-checker issues.
Triggers: 'my Rust code has lifetime errors', 'borrow checker keeps rejecting this'.
NOT for: general code style, Python/JS code."
```

同理，P2-1 = ✅ 的条件：skill 正文内有"输入示例 → 输出示例"对，或明确的正反例对比表（如 insight 的3段对话demo、radar 的"10分/1分选题"对比）。仅有"样例可以提高效果"类的说明，判定为 ⚠️。

**P2-2 · allowed-tools 设置**（工具执行型 skill 必查）
如果 skill 会调用外部工具/执行命令，检查 frontmatter 是否有 allowed-tools 限定。

判定：
- ✅ 有 allowed-tools，且只列了该 skill 实际需要的工具
- ⚠️ 有 allowed-tools 但范围过宽
- ❌ 调用外部工具但没有 allowed-tools（权限未收敛）
- N/A：纯方法论/纯建议类 skill，不执行外部操作

**P2-3 · 破坏性操作保护**（工具执行型 skill 必查）
如果 skill 会执行写入/删除/发布等不可逆操作，检查是否有前置确认机制。

判定：
- ✅ 写明"发布/删除/覆盖前必须用户确认""不许同一轮确认+执行"
- ⚠️ 有提示但不够明确，模型可能跳过确认
- ❌ 有破坏性操作但没有确认机制
- N/A：该 skill 不执行破坏性操作

**P2-4 · 方法论 skill 专项**（方法论封装型必查）
如果 skill 封装的是一套方法论/框架，检查：
- 是否有 prompt_templates（框架是骨架，模板是肉）
- 是否有 Ethical boundary（防滑向操纵）
- 是否主动声明能力边界（"给的是方向，不是定论"）

判定：
- ✅ 三项具备
- ⚠️ 部分具备
- ❌ 都没有（尤其洞察/营销/决策类 skill 的风险）
- N/A：非方法论封装类 skill

**P2-5 · 迭代友好性**
检查是否有 last-updated 字段、版本号、或 CHANGELOG。

判定：
- ✅ 有版本记录机制
- ⚠️ 有 last-updated 但无版本/变更记录
- ❌ 完全没有，迭代后无法追踪

---

## 输出格式

### 快速模式报告

```
## Skill 快速检查报告

**Skill 名称**：{name}
**检查模式**：快速（P0 核心项）

| # | 检查项 | 结果 | 问题/建议 |
|---|---|---|---|
| P0-1 | description 质量 | ✅/⚠️/❌ | {一句话} |
| P0-2 | 职责单一性 | ✅/⚠️/❌ | {一句话} |
| P0-3 | 失败分支 | ✅/⚠️/N/A | {一句话} |

**快速结论**：{✅ 核心没问题，可继续 / ⚠️ 有X处需注意 / ❌ 有X处关键缺陷，建议修复后再用}

> 需要完整检查？告诉我"完整检查"。
```

### 完整模式报告

```
## Skill 完整检查报告

**Skill 名称**：{name}
**检查模式**：完整（P0 + P1 + P2）

### P0 · 核心项
| # | 检查项 | 结果 | 问题/建议 |
|---|---|---|---|
| P0-1 | description 质量 | | |
| P0-2 | 职责单一性 | | |
| P0-3 | 失败分支 | | |

### P1 · 重要项
| # | 检查项 | 结果 | 问题/建议 |
|---|---|---|---|
| P1-1 | 配置/规则具体性 | | |
| P1-2 | 产出可判定性 | | |
| P1-3 | 正文长度 | | |
| P1-4 | 渐进式披露 | | |

### P2 · 加分项
| # | 检查项 | 结果 | 问题/建议 |
|---|---|---|---|
| P2-1 | 样例质量 | | |
| P2-2 | allowed-tools | | |
| P2-3 | 破坏性操作保护 | | |
| P2-4 | 方法论专项 | | |
| P2-5 | 迭代友好性 | | |

### 综合评分

| 层级 | 通过/总数 | 状态 |
|---|---|---|
| P0 核心 | X/3 | ✅/⚠️/❌ |
| P1 重要 | X/4 | ✅/⚠️/❌ |
| P2 加分 | X/Y（适用项）| ✅/⚠️/❌ |

**总体评级**：
- ✅ **可发布**：P0 全通过，P1 无❌
- ⚠️ **建议改进后发布**：P0 全通过，P1 有⚠️
- 🔧 **需修复**：P0 有⚠️或 P1 有❌
- ❌ **不建议使用**：P0 有❌

### 优先修复清单
{列出所有❌和重要⚠️，按 P0→P1→P2 排序，每条给具体改法}
```

---

## Rationalizations to Reject

检查过程中，不要因为以下理由放水：

- "description 虽然泛，但我能理解它的意思" → 模型触发靠的是文字匹配，不是你的理解
- "失败情况很少见，不用写" → 静默失败是最伤信任的失败，必须写死
- "这个 skill 很简单，不需要 examples" → 简单不是省略样例的理由
- "作者是专业的，规则肯定没问题" → 专业不代表具体可执行，照标准检查

---

## 来源

本 skill 的检查标准提炼自以下案例的纵深分析：
wechat-topic-radar（失败分支/配置具体性）、typefully（allowed-tools/破坏性操作）、daymade deep-research（质量门/诚实失败）、Jeffallan（渐进式披露/validator）、market-insight（方法论封装/伦理边界）、trailofbits（反偷懒清单/前置检查分级）、ask-questions（When NOT to Use/职责单一）

以上项目均为开源项目，分析仅供学习参考，设计思路归原作者所有。

---

## References

- `references/sample-report.md` — 读完检查标准想看实际报告长什么样时加载。包含一个"有典型问题的 mini-SKILL.md"和对应的完整检查报告输出，可用于校准判定尺度。

---

## CHANGELOG

### v1.1.0 — 2026-06-12
- Step 2 改为默认完整模式，移除每次必问的 AskUserQuestion（减少一轮交互）
- P2-1 判定后新增正反例参考，展示有/无样例的 description 对比和 ✅/⚠️ 分界线
- 新增 `references/sample-report.md`：真实 mini-SKILL.md 输入 + 完整检查报告输出样例
- 修正 When NOT to Use 中的笔误 `criar-skill`（葡萄牙语），改为通用表述
- P1-3 行数标准从 "<300行" 对齐官方红线 "<500行"，保留 300 作内部建议注释

### v1.0.0 — 2026-06-11
- 初始发布
- P0/P1/P2 三层检查标准，12个检查项
- 两档模式：快速（P0）/ 完整（P0+P1+P2）
- Rationalizations to Reject 反放水清单
- P2-1 加描述正反例（好/坏 description 片段）
