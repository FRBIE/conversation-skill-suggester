# conversation-skill-suggester

A practical AgentSkill that helps an assistant proactively suggest relevant skills from user conversation context, then adds a mandatory security vet step before installation.

## What It Does

- Detects capability gaps from dialogue (repetition, friction, automation intent)
- Converts requests into intent tags
- Finds and ranks candidate skills
- Returns top 1-3 recommendations with install actions
- Enforces a `skill-vetter` check before third-party skill installs

## Repository Structure

- `conversation-skill-suggester/SKILL.md` - main skill definition

## Quick Start

1. Put this skill folder under your skills directory.
2. Ensure your agent can load AgentSkills-compatible `SKILL.md` files.
3. Start a conversation that implies missing capabilities (for example: repetitive manual tasks, "can you automate this", "is there a skill for X").
4. The skill should output:
   - detected need
   - top recommendations
   - install commands
   - risk notes
   - vet step

## 30-Second Demo Script

Use this exact prompt in chat to see a quick demo:

```text
我每周都要手动汇总飞书消息和日历安排，还要发日报，太麻烦了。你能主动推荐我该装哪些 skill 吗？
```

Expected behavior in one reply:
- one-sentence detected need
- 1-3 skill suggestions ranked by fit and safety
- install command for each skill
- `skill-vetter` pre-install check
- single recommended first step

## Output Style

The skill is optimized for short, actionable responses:

- one-sentence need summary
- 1-3 ranked suggestions
- install command per suggestion
- risk line per suggestion
- explicit first-step recommendation

## Security Model

- No auto-install by default
- `skill-vetter` required before third-party installs
- unknown sources require vetting + user confirmation

## Real Conversation Examples

### Example 1 (Chinese)

User:

```text
我每次都要自己手动跑一遍网页抓取再整理成周报，太慢了。
```

Suggested output shape:

```text
Detected need: 自动化网页采集与周报生成，减少重复手工流程。
Recommended skills:
1) browser-automation - 自动执行网页采集流程，减少重复点击。
   - Install: npx skills add <skill-source>
   - Risk: low (scope limited to browser workflow)
2) report-generator - 将抓取结果结构化生成周报模板。
   - Install: npx skills add <skill-source>
   - Risk: medium (needs output template validation)
Vet step: use skill-vetter on selected skill before install
Suggested first step: 先安装 browser-automation 并跑一次最小流程。
```

### Example 2 (English)

User:

```text
I keep manually triaging support emails and creating the same Jira tickets. Can you suggest skills to automate this?
```

Suggested output shape:

```text
Detected need: automate repetitive support triage and ticket creation.
Recommended skills:
1) mail-triage - classify incoming support emails into actionable categories.
   - Install: npx skills add <skill-source>
   - Risk: medium (requires rules review for false positives)
2) jira-ticket-creator - convert validated triage items into Jira tickets.
   - Install: npx skills add <skill-source>
   - Risk: low (limited to ticket creation path)
Vet step: use skill-vetter on selected skill before install
Suggested first step: install mail-triage first and validate labels for 1 day.
```

## Notes

- The skill focuses on recommendation workflow and safety guardrails.
- You can later add connectors for ClawHub or local registries to improve candidate retrieval quality.
