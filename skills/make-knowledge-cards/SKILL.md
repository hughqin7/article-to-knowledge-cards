---
name: make-knowledge-cards
description: Turn one pasted article or one local Markdown/TXT file into 3-5 knowledge cards, each carrying a title, the core knowledge, a plain-language explanation, and an example or self-test question. Use when the user asks to extract, condense, or study an article, blog post, essay, report, transcript, or note file as knowledge cards, study cards, learning notes, summary cards, flashcards, or 知识卡片. Not for web pages, PDFs, Anki export, or GUI work.
---

# Make Knowledge Cards

Turn a single source into 3-5 cards that teach the source's most important knowledge and nothing else.

## Workflow

### 1. Get the source text

Accept exactly two kinds of input:

- Text the user pasted into the conversation.
- A local `.md` or `.txt` file path. Read it with the file-reading tool before doing anything else.

If the user gives a URL, a PDF, an image, an Office file, or a directory, stop and say this skill does not fetch web pages or read PDFs, then ask them to paste the text or save it as `.md`/`.txt`. Do not attempt a workaround, and do not guess at content you could not read.

### 2. Map the source

Read the whole source before writing any card. While reading, note:

- Which claims the source states as its main point, repeats, or develops at length.
- Which passages are one-off background, pleasantries, promotion, author biography, publication notes, or calls to action.
- Which concrete examples, numbers, or cases the source itself provides.

Length and repetition are the strongest signals of importance. Section headings are a weak signal: never turn headings into cards mechanically.

### 3. Select the knowledge points

Keep a candidate only if all of these hold:

- **Durable**: it still helps the reader a month later, away from the specific event described. Facts a reader needs only while using the thing today — schedules, prices, addresses, opening dates, headcounts, traffic or revenue projections, promotions, personnel announcements — are reference data. Compress them into one line if they matter, but do not spend a card on them.
- **The source's own claim**: the source asserts it, rather than it being general knowledge you already had.
- **Not obvious**: the reader could not have written it without reading this source. A statement that would hold for almost any article on the topic — `直达比多次换乘快`, `缓存能提速`, `沟通很重要` — is not this source's knowledge. Drop it and look for what this source actually adds.
- **One idea**: it stands alone rather than bundling two unrelated facts.

Then decide the count. The count follows from the source, never from a number the user asked for:

- 3-5 candidates: write one card each.
- More than 5: keep the 5 highest-value points and drop the rest silently. Do not compress two unrelated points into one card to fit.
- Fewer than 3: write only the cards the source supports, then add one line naming what was missing, for example `原文只支撑 2 张卡片，第 3 个知识点在文中没有展开。` Never pad with filler, restated introductions, or generic advice to reach 3.
- No supportable candidate: say the source carries no extractable knowledge point and write no cards. Do not manufacture one from background detail.

When the user names a count the source cannot support, deliver what the source supports and say plainly why the number is lower. Their requested number never justifies a weaker card.

Typical yields, to calibrate: a one-off news brief about a new transit line or a product launch usually supports at most one card and often none; a technical guide or a reference article usually supports 3-5; an argumentative essay usually supports 3-5.

Given several sources at once, handle them one at a time and ask which to start with. Never blend two sources into one card set.

### 4. Write the cards

Use this structure, and keep the four fields in this order:

```markdown
### 卡片 1：<知识点标题>
- **核心知识**：<一句话，陈述这个知识点本身>
- **简明解释**：<2-4 句，说明它为什么成立或意味着什么>
- **例子**：<原文中支撑这个知识点的例子>
```

The fourth line is either `**例子**：` or `**自测**：`, never both. Use `**例子**` whenever the source supplies a usable example, and `**自测**` only when it does not.

Rules for each field:

- **标题** names the knowledge point itself, not the topic area and not the section: `注意力是有限资源`, not `第二部分` or `关于注意力`.
- **核心知识** is one sentence. If it needs two sentences, the card is holding two points — split it or drop one.
- **简明解释** explains in your own words. Add no fact, number, name, or causal claim that the source does not contain. Restating the source's reasoning is allowed; drawing a conclusion the source never draws is not.
- **例子** reproduces a concrete case the source itself gives, keeping its facts unchanged. Never invent one, and never park a knowledge point you decided to drop in this field: an example illustrates this card's own point or it does not belong here.
- **自测** asks one question that this card alone answers. Print the question only. Never print the answer beside it, and do not volunteer answers unless the user asks for them.
- Use only numbers the source states. Do not compute derived figures, such as turning `85% vs 60%` into `a 25-point gap`.
- Write the cards in the language of the user's request, and use that language's field labels. A Chinese request gets `核心知识`, `简明解释`, and `例子` or `自测`; an English request gets `Core knowledge`, `Explanation`, and `Example` or `Self-check`. Fall back to the source's language only when the request is in no language at all.
- Make each card self-contained. No `如上文所述`, `as mentioned above`, or other references to the source's structure.
- Quote the source verbatim and mark it as a quote if you need its exact wording; otherwise paraphrase.

### 5. Check before answering

- Every fact, number, and name appears in the source as stated, with nothing computed or inferred.
- No two cards teach the same point. If two overlap, merge them or drop the weaker one.
- Each card carries exactly one knowledge point, and each title names it.
- Every card passes the durability test. No card is reference data dressed up as knowledge.
- Every card passes the not-obvious test. A card the reader could have guessed before reading is filler; replace it or drop it.
- The count is 3-5, or fewer with the missing-point line.
- Every card ends with a `**例子**` or a `**自测**` line, never both, and no self-test prints its answer.

## Output

Reply with the cards as Markdown, ordered from most to least important, prefixed by one line naming the source and the card count. Write a file only when the user asks for one, and then save Markdown.

## Out of scope

Do not fetch web pages, parse PDFs, export Anki decks, or build a GUI. When any of these is requested, name the limitation in one sentence and offer the supported path: paste the text, or save it as `.md`/`.txt`.
