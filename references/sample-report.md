# 样例：skill-checker 完整检查报告

> 本文件展示 skill-checker 对一个"有典型问题的 skill"的真实输出。
> 供用户理解报告格式，也供校准检查标准。

---

## 被检查的 Skill（输入）

```yaml
---
name: content-writer
description: "Helps you write better content for your blog."
---

# content-writer

你是一名内容写作助手。根据用户的要求，帮助撰写高质量的博客文章。

## 步骤

1. 了解用户的主题
2. 调研相关内容
3. 撰写文章
4. 优化 SEO
5. 发布到平台

保持专业风格，输出高质量内容。
```

---

## 检查报告（完整模式输出）

## Skill 完整检查报告

**Skill 名称**：content-writer
**检查模式**：完整（P0 + P1 + P2）

### P0 · 核心项
| # | 检查项 | 结果 | 问题/建议 |
|---|---|---|---|
| P0-1 | description 质量 | ❌ | 只有"帮你写更好的内容"，无触发词、无 When NOT to Use；模型不知何时该触发这个 skill |
| P0-2 | 职责单一性 | ❌ | 同时做写作+SEO+发布，是 jack-of-all-trades 反例；应拆成 content-writer / seo-optimizer / publisher |
| P0-3 | 失败分支 | ❌ | 第5步"发布到平台"调用外部操作，无任何失败处理；发布失败会静默继续 |

### P1 · 重要项
| # | 检查项 | 结果 | 问题/建议 |
|---|---|---|---|
| P1-1 | 配置/规则具体性 | ❌ | "保持专业风格""高质量内容"是口号，模型无法执行；需要：字数范围/段落结构/禁用词/目标读者定义 |
| P1-2 | 产出可判定性 | ❌ | 无任何验收标准；什么叫"写好了"？需要：字数/可读性分/关键词密度阈值等 |
| P1-3 | 正文长度 | ✅ | 22行，简洁（问题是内容不够，不是太长） |
| P1-4 | 渐进式披露 | N/A | 正文 <150行，不需要 references/ |

### P2 · 加分项
| # | 检查项 | 结果 | 问题/建议 |
|---|---|---|---|
| P2-1 | 样例质量 | ❌ | 无任何输入/输出样例 |
| P2-2 | allowed-tools | ❌ | 第5步执行发布操作，但无 allowed-tools；权限完全未收敛 |
| P2-3 | 破坏性操作保护 | ❌ | "发布到平台"是不可逆操作，无前置确认机制 |
| P2-4 | 方法论专项 | N/A | 非方法论封装类 |
| P2-5 | 迭代友好性 | ❌ | 无版本号、无 last-updated、无 CHANGELOG |

### 综合评分

| 层级 | 通过/总数 | 状态 |
|---|---|---|
| P0 核心 | 0/3 | ❌ |
| P1 重要 | 1/4 | ❌ |
| P2 加分 | 0/4（适用项）| ❌ |

**总体评级**：❌ 不建议使用（P0 有❌）

### 优先修复清单

**P0-1 · description（必须先修）**
改法：`"Writes blog articles given a topic. Triggers: 'write a blog post about X', '帮我写篇关于X的文章'. NOT for: SEO optimization (use seo-optimizer), publishing (use publisher)."`

**P0-2 · 职责单一（必须先修）**
改法：拆成3个 skill：`content-writer`（只写文章）+ `seo-optimizer`（只优化SEO）+ `publisher`（只发布），各自在 description 里互指。

**P0-3 · 失败分支（必须先修）**
改法：在第5步前加"发布前必须用户确认；发布失败时报错并停止，禁止重试"。

**P1-1 · 规则具体化**
改法：写死参数，如"字数800-1500字 / 每段不超过150字 / 禁用被动语态 / 目标读者=有3年经验的产品经理"。

**P2-2 + P2-3 · allowed-tools + 破坏性保护**
改法：frontmatter 加 `allowed-tools: WebSearch, Write`（去掉发布工具或单独拆 publisher skill）。

---

## 对比：修复后的 description（P0-1 变化）

**修复前** ❌：
```
description: "Helps you write better content for your blog."
```

**修复后** ✅：
```
description: "Writes structured blog articles (800-1500 words) given a topic and target audience.
Triggers: 'write a blog post about X', '写一篇关于X的文章', 'draft an article on Y'.
NOT for: SEO optimization (use seo-optimizer), publishing to platforms (use publisher), social media posts."
```
