---
name: card-design
description: How to design well-formed spaced-repetition flashcards in PeachCard. Use this skill whenever you are about to call `create_card` or `create_cards_batch` on the peachcard MCP server, or whenever the user asks to "add cards", "make flashcards", "ankify", or similar. Covers atomicity, note-type selection (basic / basic_reverse / cloze), cloze syntax, HTML formatting (the #1 gotcha — use `<br>` not `\n` for line breaks), and common anti-patterns from Wozniak's 20 rules and Matuschak/Nielsen's modern SRS writing. Examples in `examples.md`.
---

# Card design for PeachCard

This skill is the reference for writing PeachCard flashcards that the user will actually retain. Synthesised from Wozniak's "20 rules of formulating knowledge" (1999), Matuschak & Nielsen's *Augmenting Long-term Memory*, and modern SRS practice (AnKing, language-learning, programming).

Read the core principles and HTML section every time. Reach for `examples.md` when you need concrete before/after cases (especially for cloze + list patterns).

## Core principles

1. **One idea per card** (atomicity). If a card can be split into two, split it. Cards with multi-fact answers fail in SRS because the user can recall one fact and not the other — there's no way to record partial recall.

2. **Shortest unambiguous prompt.** Cards should be answerable in under 10 seconds. Cut every word that isn't load-bearing. Long cards get Again-spammed and abandoned.

3. **Keep backs short.** Ideally one sentence, never a paragraph. Pick the single most important fact. If there are three facts, write three cards.

4. **Phrase fronts as questions**, not labels. *"What does FSRS stand for?"* beats *"FSRS"*. The question form forces real retrieval; a bare label invites pattern-matching on the surrounding context.

5. **Connect new cards to old ones.** Orphan cards decay fastest. When introducing a new topic, write 3+ related cards and tag them under a shared hierarchy (e.g. `rust::traits`). Tags use `::` for nesting.

6. **Match the user, not the source.** Don't copy textbook prose verbatim — source-specific phrasing creates a shortcut that doesn't transfer. Rewrite each card in the user's own voice and at their level:
   - Use vocabulary they've already shown they know (from existing cards, current conversation, stated background).
   - For a beginner: no jargon without definition. For an expert: skip the basics.
   - Anchor new facts to concepts they already have cards for. That's what creates the "neighbour" relationships that prevent orphan-decay.

## Note type selection

- **`basic`** — the default. Use when the prompt has ONE clear answer and reversing wouldn't be useful.
  Example: *"When was Rust 1.0 released?"* → *"2015"*

- **`basic_reverse`** — ONLY for short symmetric pairs where both directions match real recall situations.
  Good: vocabulary (*"cat"* ↔ *"gato"*), symbols (*"H₂O"* ↔ *"water"*), short formulas, term ↔ acronym.
  BAD: concept → description pairs ("Point 1: Build Motivation" / long paragraph). Recalling a title from a paragraph is a useless memory task.

- **`cloze`** — hide a key term embedded in a sentence. Best for definitions, named concepts, key numbers, steps in a sequence.
  Atomic clozes (hide one term) beat full-sentence clozes:
  ✓ `The {{c1::mitochondria}} is the powerhouse of the cell.`
  ✗ `{{c1::The mitochondria is the powerhouse of the cell.}}`

### Cloze syntax (exact format)

- `{{c1::answer}}` — hides "answer", shows `[...]` on the front.
- `{{c1::answer::hint}}` — hides "answer", shows `[hint]` on the front.
- Multiple `{{c1::...}}` in one sentence repeat the same blank (still one card).
- `{{c1::...}}` and `{{c2::...}}` create separate cards (one per unique cN).
- Example: `The capital of {{c1::France}} is {{c2::Paris}}` → 2 cards.

## HTML formatting (the #1 gotcha)

Note fields (`front`, `back`, `text`) accept a safe subset of HTML. **Plain `\n` does NOT render as a line break** — HTML collapses whitespace. This is the most common mistake; if you want vertical structure, use HTML.

### When to reach for HTML

- Listing items in a card → use `<br>` between items, or `<ul><li>...</li></ul>` / `<ol>` for proper lists.
- Emphasising the key term in a longer sentence → `<strong>` (sparingly, max one per card).
- Inline code or identifiers → `<code>foo</code>`.
- Code blocks → `<pre><code>...</code></pre>`.
- Chemistry / math → `<sub>` / `<sup>` (`H<sub>2</sub>O`, `x<sup>2</sup>`).

### Cloze + list pattern (apply this to multi-item clozes)

If you're writing a cloze card that enumerates several items, **always insert `<br>` between the items.** Otherwise the items run together as an inline semicolon-list, which is unreadable during review.

**BAD** (inline, no structure):
```
SMART Recovery's 4-Point Program: (1) {{c1::build motivation}}; (2) {{c2::cope with urges}}; (3) {{c3::manage thoughts}}; (4) {{c4::achieve lifestyle balance}}.
```

**GOOD** (vertical, scannable):
```
SMART Recovery's 4-Point Program:<br><br>1. {{c1::Build motivation}}<br>2. {{c2::Cope with urges}}<br>3. {{c3::Manage thoughts}}<br>4. {{c4::Achieve lifestyle balance}}
```

### Allowed tags

`p, br, ul, ol, li, blockquote, hr, strong, em, b, i, u, s, code, pre, sub, sup, mark, span, kbd, a, img`. Everything else is stripped by the server-side sanitiser.

### Don'ts

- **No inline styles.** No `style=`, no colors, no fonts. Let the deck CSS handle appearance.
- **No images yet via MCP** — `<img>` rendering only works for the server's own CDN domain, and image upload via MCP isn't wired up. Don't include `<img>` unless the user explicitly provides a CDN URL.
- **Don't reach for HTML when plain text is fine.** Plain text is the default. Reach for HTML when structure genuinely helps (lists, code, sub/sup).

## Anti-patterns — refuse to write cards like these

- **Wall-of-text answer.** Refactor into 3–8 atomic cards.
- **Memorising lists/sets verbatim.** Sets are nearly impossible to recall completely. Use overlapping clozes per item; learn the ordering separately if needed.
- **Yes/no answers.** A binary answer is a code smell — the card produces 50% accuracy by accident. Reword to require richer recall.
- **Reverse cards by default.** Doubles review for half the value when only one direction matters.
- **Overly specific prompts** that only match the textbook context. Reframe to match real recall situations.

## Advanced techniques

- **Context cues** — prefix prompts with a tiny domain handle (`Rust:`, `bioch:`) to disambiguate without bloat.
- **Date-stamp volatile facts** — `As of 2026-Q2, AWS Lambda max memory is {{c1::10240MB}}.` Future-you will know to verify.
- **Encode for the recall situation** (encoding specificity). Will the user need this while writing code? Make the card look like code. While speaking a language? Add audio context. The prompt's form should match the situation where the knowledge will be used.
- **Add source** on the back (URL, book + page) for non-trivial facts. Costs 10 seconds; saves 30 minutes later.
- **Question variants** — approaching the same fact from 2–3 different angles each builds a separate retrieval path. Not redundant; reinforcing.

## Workflow when the user says "add cards on X"

1. **Check understanding.** If the topic is complex and you're not sure the user understands it yet, do a brief check before generating cards. SRS retains comprehension; it doesn't create it.
2. **Find their level.** Skim recent conversation, existing tags, and at least one existing card to gauge vocabulary and background.
3. **Plan the card set.** What are the 3–10 atomic facts worth retaining? Each fact = one card. Avoid topics with only one card (orphans).
4. **Pick note type per fact.** Most are `basic`. Reach for `cloze` when there's a key term in a sentence. Avoid `basic_reverse` unless the pair is genuinely symmetric.
5. **Draft, then prune.** Write the cards, then ask: *could this be shorter? could this be split?* For lists in cloze: did I use `<br>`?
6. **Tag consistently.** Use the user's existing tag hierarchy if they have one. Lowercase, `::`-separated.
7. **Use `create_cards_batch`** when adding 3+ cards to the same deck. One call instead of N.

When in doubt, see `examples.md` for concrete before/after patterns.
