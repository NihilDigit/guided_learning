# Guided Reading

A [Claude Code](https://claude.com/claude-code) skill for interactive, guided study of textbooks, papers, and technical books.

## Problem

Two common failure modes when studying dense material:

1. **Passive reading** — you lose focus and skim without absorbing
2. **Passive listening** — someone explains it to you and you *think* you understand, but you don't

## Solution

**Step 0: Material prep** — Automatically converts your source material (PDF, EPUB, DOCX, HTML) to clean Markdown via `markitdown`. Detects unsupported formats (scanned PDFs, images) and tells you upfront.

Then a 5-step interactive cycle per section:

1. **Read** — Claude reads the entire section
2. **Distill** — Identifies 3-5 core learning points
3. **Guide** — Poses questions first, then feeds source text in small segments
4. **Dialogue** — You respond; Claude probes, corrects, or advances
5. **Notes** — Claude guides you to articulate what you learned, then compiles structured notes

## Key Design Choices

- **Questions before text** — you read with purpose, not passively
- **Never dumps explanations** — if you're wrong, it asks smaller sub-questions instead of giving the answer
- **Guided note-taking** — you say what you learned; Claude organizes it. Not the other way around
- **Anti-drift** — unit boundary checks, auto-pause after 3 rounds on the same concept, no jumping ahead
- **Language-adaptive** — responds in whatever language you use

## Install

Copy `SKILL.md` to your Claude Code skills directory:

```bash
# Create skill directory
mkdir -p ~/.claude/skills/guided-reading

# Copy the skill file
cp SKILL.md ~/.claude/skills/guided-reading/SKILL.md
```

## Usage

In Claude Code, say things like:

- "Let's study chapter 3"
- "Read the next section"
- "Continue studying"
- `/guided-reading`

During a session:

| Command | Action |
|---------|--------|
| "continue" / "next" | Advance to next text segment |
| "don't understand" | Re-explain from a different angle |
| "skip" | Skip current unit |
| "notes" / "summarize" | Compile notes for current progress |
| "pause" | Save progress and stop |

## License

[MIT](LICENSE)
