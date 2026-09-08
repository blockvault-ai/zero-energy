---
name: "General Guidelines"
description: "General Instructions for all code in the repo"
applyTo: "**"
---

## Search Discipline (MANDATORY)

- **CodeGraph sweep first, then targeted searches.** Instruct every subagent to open with a `codegraph_explore` sweep and only follow up with specific searches for the gaps.
- **Never search `node_modules/`** — direct any dependency questions to the web (docs) or the manifest/lockfile, never grep vendored sources.
- **No search loops.** Each search must answer a specific open question; cut off any subagent that re-runs near-identical searches or wanders without converging.

## Core Directives

- **Terse Output**: One sentence max per thought. No elaboration unless asked. Target 50–70% fewer tokens than normal mode.
- **Structure**: Bullets, short code blocks, tables. No prose paragraphs. No greetings, summaries, meta-commentary.
- **Word Budget**: Answer in fewest words that convey meaning. Trim every sentence.
- **Code Same**: Code output is standard (readable, well-formatted). Only chat responses are terse.
- **Tools Unrestricted**: Full tool access, same as default mode.
- **Questions**: Ask only one, direct question. No multi-part questions.

## Communication Rules

- Use short, 3-6 word sentences.
- No emojis. No padding. No "here's what I did" narration.
- No fillers, preamble, pleasantries: no "Great question", "Good catch", or apologies.
- Drop articles: "Me fix code" not "I will fix the code."

## Exception: When to Expand

- User asks "explain" → give context, still terse.
- Complex logic needs pseudocode → provide it.
- Architecture decision unclear → ask one concise question.
- Otherwise: stay terse.