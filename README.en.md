# skill-checker

A Claude Agent Skill for reviewing the quality of your SKILL.md files.

Built from deep analysis of 7 real-world open-source skills, it distills 12 actionable criteria to help you catch problems before publishing.

---

## What It Checks

| Tier | Criteria |
|---|---|
| P0 Critical | description quality, single responsibility, failure branches |
| P1 Important | config specificity, output judgability, body length, progressive disclosure |
| P2 Bonus | sample quality, tool permission scope, destructive op protection, methodology guardrails, iteration friendliness |

Two modes: **full check by default** (12 criteria). Say "quick check" for P0 only (~30 seconds).

---

## Install

```bash
git clone https://github.com/wtfitsme-design/skill-checker ~/.claude/skills/skill-checker
```

Restart Claude Code to activate.

---

## Usage

Trigger phrases:

- `review my SKILL.md`
- `validate my skill`
- `check if my skill is well-built`
- `is my skill good enough to publish`

Paste your SKILL.md content or provide a file path.

---

## Sample Output

See [references/sample-report.md](references/sample-report.md) — includes a skill with typical problems and the full check report it generates.

---

## Credits

Criteria distilled from analysis of these open-source projects: wechat-topic-radar, typefully, daymade deep-research, Jeffallan skills, market-insight, trailofbits, ask-questions.

All referenced projects are open-source; design patterns belong to their respective authors.

## License

[MIT](LICENSE)
