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

## Notes

- The skill focuses on recommendation workflow and safety guardrails.
- You can later add connectors for ClawHub or local registries to improve candidate retrieval quality.
