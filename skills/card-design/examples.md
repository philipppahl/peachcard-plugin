# Card design examples — before / after

Concrete cases. Open this when you need a pattern you haven't internalised yet.

---

## 1. Single fact that should be `basic`

**Source:** "Rust 1.0 was released on May 15, 2015."

**BAD** (label, not a question):
```
Front: Rust 1.0
Back: Released May 15, 2015.
```

**GOOD**:
```
Front: When was Rust 1.0 released?
Back: 15 May 2015.
```

---

## 2. Concept → description: do NOT use basic_reverse

**Source paragraph:** "Encoding specificity (Tulving & Thomson): memories are most reliably retrieved when the retrieval context matches the encoding context."

**BAD** (basic_reverse on title/description):
```
Front: Encoding specificity
Back: Memories are most reliably retrieved when the retrieval context matches the encoding context (Tulving & Thomson).
```
The reverse card asks you to recall "encoding specificity" given a paragraph — useless memory task.

**GOOD** (basic, framed as a use case):
```
Front: Why does practising recall in the same context you'll test in help retention?
Back: Encoding specificity — memories retrieve best when retrieval context matches encoding context.
```

**ALSO GOOD** (cloze with the key term hidden):
```
Cloze: {{c1::Encoding specificity}} (Tulving & Thomson): memories retrieve most reliably when retrieval context matches encoding context.
```

---

## 3. Vocabulary pair — basic_reverse is correct

**Source:** Spanish vocabulary

**GOOD** (real symmetric pair):
```
basic_reverse
Front: cat
Back: gato
```
Both directions are useful (you need to recognise *and* produce the word in real situations).

---

## 4. List inside a single cloze — the #1 HTML gotcha

**Source:** "SMART Recovery's 4-Point Program: build motivation, cope with urges, manage thoughts, achieve lifestyle balance."

**BAD** (inline, runs together as semicolons during review):
```
cloze
text: SMART Recovery's 4-Point Program: (1) {{c1::build motivation}}; (2) {{c2::cope with urges}}; (3) {{c3::manage thoughts}}; (4) {{c4::achieve lifestyle balance}}.
```

**GOOD** (`<br>` between items so they stack vertically):
```
cloze
text: SMART Recovery's 4-Point Program:<br><br>1. {{c1::Build motivation}}<br>2. {{c2::Cope with urges}}<br>3. {{c3::Manage thoughts}}<br>4. {{c4::Achieve lifestyle balance}}
```

Same content, renders as a scannable list instead of a wall of semicolons. Apply this any time a single cloze enumerates ≥ 3 items.

---

## 5. List as an explicit `<ol>` (also valid)

**Source:** "SMART's four Disputing questions: 1) Do I have evidence this isn't true? 2) Is there another way to view this? 3) Is it rational under calm conditions? 4) Is it helpful to me?"

**GOOD** (basic with an ordered list HTML):
```
basic
Front: What are SMART's four Disputing questions?
Back: <ol><li>Do I have evidence this isn't always true?</li><li>Is there another way to view this?</li><li>Is it rational under calm conditions?</li><li>Is it helpful to me?</li></ol>
```

Use `<ol>` or `<ul>` when the items are sentences or longer than a few words. Use `<br>` separation when the items are short.

---

## 6. Splitting one long card into atomic cards

**Source paragraph:** "FSRS models memory with three variables: stability (the time until recall probability drops to the target), difficulty (per-card, 1–10, mean-reverting), and retrievability (current recall probability)."

**BAD** (one card, three facts):
```
basic
Front: What three variables does FSRS use to model memory?
Back: Stability (time until recall probability drops to target), difficulty (per-card 1–10, mean-reverting), and retrievability (current recall probability).
```

**GOOD** (four atomic cards):
```
1. basic
   Front: What three variables does FSRS use to model memory?
   Back: Stability, difficulty, retrievability.

2. cloze
   text: In FSRS, {{c1::stability}} is the time until recall probability drops to the target retention.

3. cloze
   text: In FSRS, {{c1::difficulty}} is a per-card value (1–10) that mean-reverts toward the population average.

4. cloze
   text: In FSRS, {{c1::retrievability}} is the current recall probability — derived from elapsed time and stability.
```

Total review time per card drops, partial recall is now expressible, and the three concepts can decay at their own rates.

---

## 7. Code question — encode for the recall situation

**Source:** Python `map` returns a lazy iterator.

**BAD** (verbal definition, doesn't match how you'll need it):
```
basic
Front: What does Python's map() return?
Back: A lazy iterator that applies the function to each element of the iterable.
```

**GOOD** (encoded as the situation where you'll need it):
```
basic
Front: What does this print?<br><code>r = map(lambda x: x*2, [1, 2, 3])<br>print(list(r))<br>print(list(r))</code>
Back: First line: <code>[2, 4, 6]</code>. Second: <code>[]</code> — map returns an iterator, exhausted after one pass.
```

Now the card teaches you the gotcha at the moment you'd encounter it (looking at code).

---

## 8. Avoid yes/no

**BAD**:
```
basic
Front: Is FSRS the default scheduler in PeachCard?
Back: Yes.
```
50% recall by accident. Doesn't teach anything.

**GOOD**:
```
basic
Front: Which scheduler does PeachCard use, and why FSRS over SM-2?
Back: FSRS. It models memory with stability/difficulty/retrievability instead of SM-2's ease factor, which improves long-term interval accuracy and avoids "ease hell".
```

---

## 9. Date-stamp volatile facts

**Source (current):** "As of mid-2026, AWS Lambda allows up to 10 GB of memory per function."

**GOOD**:
```
cloze
text: As of 2026-Q2, AWS Lambda max memory per function is {{c1::10 GB (10240 MB)}}.
```

When the user reviews this in 2028 and the limit has changed, the date stamp tells them to verify.

---

## 10. Add source when the user will want it

**Source:** A specific page in a textbook.

**GOOD**:
```
basic
Front: What's the "minimum information principle" in Wozniak's 20 rules?
Back: A card should be as simple as it can be without losing meaning. One fat question becomes nine lean ones.<br><br><small>— Wozniak (1999), Rule 4. <a href="https://super-memory.com/articles/20rules.htm">source</a></small>
```

Small `<small>` for the citation keeps it visually de-emphasised but available when you want to go back to the source.
