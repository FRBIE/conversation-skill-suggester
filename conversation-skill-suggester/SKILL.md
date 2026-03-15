---
name: conversation-skill-suggester
description: Proactively identify missing capabilities from live user conversation and recommend the most relevant skills with clear install actions. Use when the user describes repeated pain points, asks "can you do X", requests automation, or discusses workflows that likely need new skills.
---

# Conversation Skill Suggester

Analyze the latest user request and recent chat context, then propose skill recommendations before the user explicitly asks.

## Run This Workflow

1. Detect trigger signals from user language:
- Repetition: "every time", "经常", "总是要"
- Friction: "麻烦", "太慢", "手动"
- Capability gap: "你能不能", "有没有办法", "能自动吗"
- Tool mismatch: user asks for integrations not currently available

2. Convert signals into capability intents:
- Summarize the user need in one sentence
- Extract 3-6 intent tags (e.g., `calendar`, `email`, `browser automation`, `reporting`, `feishu`)

3. Search candidate skills (in order):
- Local installed skills first
- Workspace/local registries (e.g., ClawHub mirrors)
- Public registries only when needed

4. Rank top candidates using this score:
- Need fit (0-5)
- Safety/risk (0-5, higher is safer)
- Setup effort (0-5, higher is easier)
- Maintenance burden (0-5, higher is lower burden)
- Final score = `fit*0.45 + safety*0.30 + setup*0.15 + maintenance*0.10`

5. Present recommendations with actionability:
- Give top 1-3 skills only
- For each: what it solves, why now, risk note, install command
- End with one suggested default: "I recommend installing X first"

## Output Template

Use this compact format:

- Detected need: <one-sentence need>
- Recommended skills:
  1) <skill-name> - <why it fits in one line>
     - Install: `<command>`
     - Risk: <low/medium/high + one reason>
  2) <skill-name> - <why it fits in one line>
     - Install: `<command>`
     - Risk: <low/medium/high + one reason>
- Suggested first step: <single best next action>

## Guardrails

- Do not recommend more than 3 skills per turn.
- Prefer safer, smaller-scope skills over powerful but risky skills.
- If confidence is low, ask one clarifying question before recommending.
- Never auto-install without explicit user confirmation.
- For unknown third-party skills, run a security vet step before install.

## Chinese Interaction Style

When user speaks Chinese, respond in Chinese and keep recommendation lines short and practical.
