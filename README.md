# peachcard-plugin

A Claude Code plugin for [PeachCard](https://peachcard.peachstone.ai) — a spaced-repetition flashcard app built on FSRS (the modern open-source SRS algorithm). Install it once and Claude can create, review, and search your flashcards from inside any conversation.

## What it does

- **Registers the PeachCard MCP server.** No editing `.claude.json`.
- **Ships a `card-design` skill** so Claude writes well-formed cards by default: atomic, short, list-aware (the `<br>` gotcha is the #1 thing the skill teaches), and tailored to your level instead of textbook prose.
- **MCP tools** for creating decks, adding cards (single or batched), reviewing, searching, and seeing stats.

## Install in Codex

Add this repository as a Codex plugin marketplace, then install the PeachCard plugin:

```
codex plugin marketplace add git@github.com:philipppahl/peachcard-plugin.git --ref main
codex plugin add peachcard@peachcard
```

Start a new Codex thread after installation so the PeachCard MCP tools and `card-design` skill are loaded.

## Install

In any Claude Code session (CLI, desktop GUI, or IDE extension):

```
/plugin install philipppahl/peachcard-plugin#main
```

Pick **User** scope when prompted so it works in every project.

Then authenticate the MCP server (one-time, browser opens):

```
/mcp
```

That's it. Auto-updates ship within a week of any push to `main`.

## What's in it

```
peachcard-plugin/
├── .claude-plugin/plugin.json   # manifest
├── .mcp.json                    # registers the peachcard MCP server
├── skills/card-design/
│   ├── SKILL.md                 # design principles, note types, HTML, anti-patterns
│   └── examples.md              # before/after patterns for the tricky cases
└── README.md
```

## Usage

Once installed, just describe what you want to learn and Claude does the rest:

> "Add some cards on the SMART Recovery 4-point program."

Claude loads the `card-design` skill (because creating cards triggers it), follows the principles, picks the right note type for each fact, applies `<br>` where lists need it, and writes them via the MCP tools. You review in the web app at https://peachcard.peachstone.ai or via `mcp__peachcard__review_card`.

To study from inside Claude:

> "Show me what's due in the SMART Recovery deck."

## Credits

Card design principles synthesised from:
- Piotr Wozniak, [*Effective learning: Twenty rules of formulating knowledge*](https://super-memory.com/articles/20rules.htm) (1999)
- Michael Nielsen, [*Augmenting Long-term Memory*](https://augmentingcognition.com/ltm.html)
- Andy Matuschak & Michael Nielsen, [*How can we develop transformative tools for thought?*](https://numinous.productions/ttft/)
- The cognitive science of retrieval practice and desirable difficulties (Bjork & Bjork, Roediger & Karpicke)

## License

MIT.
