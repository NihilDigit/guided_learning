---
name: guided-reading
description: >
  Guided reading workflow for studying textbooks, papers, and technical books. Trigger when user
  says "start studying", "guided read", "read next section", "continue studying", or specifies
  a chapter/section to learn. Do NOT use for quick lookups or information retrieval.
user-invocable: true
---

# Guided Reading

An interactive learning workflow that prevents two failure modes: passive reading (losing focus) and passive listening (illusion of understanding).

## Workflow Overview

Each section follows a 5-step cycle:

1. **Read** — Read the entire section source text
2. **Distill** — Identify 3-5 core learning points
3. **Guide** — Pose questions first, then feed source text in segments (max ~200 words each)
4. **Dialogue** — User responds after reading; probe, correct, or advance
5. **Notes** — After completing a section, guide the user to articulate notes, then compile and save to the project `notes/` directory

## Step 0: Material Preparation

Before any learning begins, the source material must be converted to clean Markdown. This step runs once per material, not per section.

### Supported formats and conversion

| Format | Approach |
|--------|----------|
| `.md` / `.txt` | Use directly, no conversion needed |
| `.pdf` (text-based) | Extract target pages with `qpdf`, then convert with `markitdown`. Verify output is readable text, not garbled |
| `.html` | Convert with `markitdown` |
| `.epub` | Convert with `markitdown` |
| `.docx` | Convert with `markitdown` |

### Unsupported formats

If the material is in a format that cannot be reliably converted to text (e.g. scanned image PDF, screenshots, handwritten notes, video), **explicitly inform the user**:

- State which format was detected and why it cannot be processed
- Suggest alternatives (e.g. "This PDF appears to be scanned images. You could run OCR on it first, or provide a text-based version")
- Do NOT silently skip or hallucinate content from unreadable material

### Conversion workflow

1. User provides the material (file path, URL, or pasted text)
2. Detect format. If unsupported, inform user and stop
3. Convert to Markdown and save to project directory (e.g. `notes/ch01_raw.md`)
4. Quick-verify: read the first ~50 lines of output to confirm it's readable, not garbled
5. If garbled or mostly empty, inform the user that conversion failed and suggest alternatives
6. Once clean Markdown is ready, ask user which section to start with

## Detailed Rules

### Steps 1-2: Read & Distill

- Read the entire section at once; do not feed text to the user while reading
- Distill core concepts and classify: must master / good to know / skippable
- If a section is too long (>3000 words), split into multiple learning units

### Step 3: Guide

- **Questions before text**: Pose 2-3 questions at the start of each learning unit so the user reads with purpose
- **Segment feeding**: Present source text as plain text (do NOT use blockquotes `>` — they render poorly in terminals), each segment ~100-200 words
- **Complete code**: Never truncate code blocks; always present them in full
- **Language**: Always respond in the same language the user is using. If the source text is in a different language from the user's, translate it into the user's language, keeping only domain-specific terms in their original form

### Step 4: Dialogue

- After the user responds, first assess whether their understanding is correct
- Correct: briefly confirm (e.g. "Right."), then advance to the next segment
- Partially correct: affirm the correct parts, correct the wrong parts
- Wrong: do NOT give the answer directly; guide with smaller sub-questions
- User says "I don't understand" / "confused": ask what specifically is unclear; locate the most fundamental point of confusion
- **Forbidden**: dumping large blocks of explanation at once. Do not explain what the user hasn't asked about

### Step 5: Notes (Guided)

Do NOT write notes for the user directly. Guide them to articulate note content through questions:

- Ask "What core concepts did you learn in this section?"
- After the user lists points, probe for gaps: "There was also something about X — how do you understand it?"
- Correct imprecise wording when the user's phrasing is off
- Once all key points are covered, compile the user's responses into structured notes and save to the `notes/` directory
- Note format: heading hierarchy matches source structure, bullet points for key ideas, code in code blocks
- Only record content the user **actually understood** — never include material not yet covered
- After saving notes, inform the user and ask whether to continue to the next section

## Anti-Drift Mechanisms

- **Unit boundaries**: After each learning unit, explicitly mark completion and ask "Any more questions? If not, moving on"
- **Self-check**: If 3 consecutive dialogue rounds are spent explaining the same concept, pause and ask the user if a different angle would help
- **Progress tracking**: At the start of each section, state "Currently at X.Y.Z, the core of this section is..."
- **No jumping ahead**: Do not introduce content from later sections unless the user explicitly asks
- **One at a time**: Advance only one concept at a time; confirm understanding before moving to the next

## User Commands

- "continue" / "next" → advance to the next text segment
- "don't understand" / "explain again" → re-explain the current concept from a different angle
- "skip" → mark current content as "skipped", move to the next learning unit
- "summarize" / "notes" → immediately compile learned content into notes
- "pause" → save progress, end current session
