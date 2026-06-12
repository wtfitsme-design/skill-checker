# skill-checker

一个用于检查 Claude Agent Skill 构建质量的 skill。

基于对 7 个真实开源 skill 案例的纵向深挖与横向对比，提炼出 12 条可执行的检查标准，帮你在发布前发现问题。

---

## 功能

| 层级 | 检查项 |
|---|---|
| P0 核心 | description 质量、职责单一性、失败分支处理 |
| P1 重要 | 配置具体性、产出可判定性、正文长度、渐进式披露 |
| P2 加分 | 样例质量、权限收敛、破坏性操作保护、方法论专项、迭代友好性 |

两档模式：**默认完整检查**（12条）。时间紧时说"快速检查"只跑 P0（3条，约30秒）。

---

## 安装

```bash
git clone https://github.com/wtfitsme-design/skill-checker ~/.claude/skills/skill-checker
```

重启 Claude Code 后生效。

---

## 使用

直接对 Claude 说：

- `帮我检查这个skill`
- `我写的skill有没有问题`
- `我写的skill好不好`
- `检查一下我的skill`

然后粘贴 SKILL.md 内容，或提供文件路径。

---

## 输出示例

见 [references/sample-report.md](references/sample-report.md)，包含一个有典型问题的 skill 和对应的完整检查报告。

---

## 来源

检查标准提炼自以下开源项目的分析：wechat-topic-radar、typefully、daymade deep-research、Jeffallan skills、market-insight、trailofbits、ask-questions。

以上项目均为开源项目，分析仅供学习参考，设计思路归原作者所有。
