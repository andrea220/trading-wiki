# Trading Wiki — Schema & Operating Instructions

This is a personal knowledge base about **trading and financial markets**, maintained by an LLM. You (the LLM) own the `wiki/` directory entirely. You read from `raw/` but never modify it. The human curates sources and asks questions; you do the summarizing, cross-referencing, filing, and bookkeeping.

---

## Directory Layout

```
trading-wiki/
├── CLAUDE.md            ← this file
├── raw/                 ← immutable source documents (human-managed)
│   ├── articles/        ← web clippings in markdown (e.g., from Obsidian Web Clipper)
│   ├── papers/          ← PDFs and research papers
│   └── notes/           ← personal trade journals, meeting notes, reflections
└── wiki/                ← LLM-maintained markdown files
    ├── index.md         ← master catalog of all wiki pages (update on every ingest)
    ├── log.md           ← append-only chronological record of all operations
    ├── overview.md      ← high-level synthesis of the current state of knowledge
    ├── concepts/        ← trading concepts, macro themes, phenomena
    ├── instruments/     ← asset classes, specific instruments, markets
    ├── strategies/      ← trading strategies and approaches
    ├── people/          ← traders, researchers, authors, fund managers
    ├── events/          ← notable market events, crises, regime changes
    ├── sources/         ← one summary page per ingested source
    └── synthesis/       ← comparison tables, analyses, answers to complex queries
```

Create any missing directories as needed. Never delete raw source files.

---

## Page Format

Every wiki page starts with YAML frontmatter:

```yaml
---
type: concept | instrument | strategy | person | event | source | synthesis
tags: [tag1, tag2]
sources: [raw/articles/foo.md, raw/papers/bar.pdf]   # only on non-source pages
created: YYYY-MM-DD
updated: YYYY-MM-DD
---
```

After the frontmatter, write normal markdown. Use `[[WikiPageName]]` for internal links (Obsidian-style wikilinks). Be specific: prefer `[[MomentumFactor]]` over a vague mention. Every page should link to at least 2-3 related pages.

**Naming convention**: `PascalCase.md` for all wiki pages (e.g., `MomentumFactor.md`, `WarrenBuffett.md`, `BlackMonday1987.md`).

---

## Page Types

### `source` — tracked in log.md, NOT as a separate page

**Do not create `.md` files for sources.** Any `.md` file in the vault appears as a node in Obsidian's graph, including files in `wiki/sources/`. Since the graph must contain only concept nodes, source summaries are recorded exclusively in `wiki/log.md` — not as standalone pages.

The `sources:` frontmatter field on concept pages (a list of raw file paths) provides all the traceability needed without creating extra nodes.

Each ingest entry in `wiki/log.md` should include:
- **Source**: file path in `raw/`, type, author, date if known
- **Summary**: 3–5 sentence synthesis of the key ideas
- **Key claims**: bulleted list of the most important factual assertions
- **Contradictions / tensions**: anything that conflicts with existing wiki pages
- **Gaps / open questions**: what the source leaves unanswered

### `concept` — trading concepts and phenomena
Lives in `wiki/concepts/`. Examples: `MomentumFactor.md`, `CarryTrade.md`, `VolatilitySurface.md`, `LiquidityPremium.md`, `MarketMicrostructure.md`.
Captures: definition, intuition, evidence base, how it interacts with other concepts, practitioner debate, edge cases.

### `instrument` — asset classes and specific markets
Lives in `wiki/instruments/`. Examples: `SPX.md`, `UST10Y.md`, `CrudeOilFutures.md`, `Bitcoin.md`, `VIX.md`.
Captures: what it is, key drivers, typical behavior in different regimes, notable relationships with other instruments.

### `strategy` — trading approaches
Lives in `wiki/strategies/`. Examples: `StatisticalArbitrage.md`, `TrendFollowing.md`, `VolatilityArbitrage.md`, `GlobalMacro.md`.
Captures: core logic, typical instruments, entry/exit rules as described in sources, known edge, known risks, evidence for/against, who uses it.

### `person` — key figures
Lives in `wiki/people/`. Examples: `GeorgeSoros.md`, `EugeneKlein.md`, `EdSeykota.md`.
Captures: background, known strategies or frameworks, notable trades or calls, published work.

### `event` — market events and regime shifts
Lives in `wiki/events/`. Examples: `BlackMonday1987.md`, `2008FinancialCrisis.md`, `Covid2020Crash.md`, `2022BondCrash.md`.
Captures: what happened, timeline, causes (debated and consensus), impact on strategies and instruments, lessons cited in the literature.

### `synthesis` — analyses and complex answers
Lives in `wiki/synthesis/`. Created when a query produces a valuable answer worth preserving. Examples: `MomentumVsReversal.md`, `RateRisingRegimePlaybook.md`.
Captures: the question it answers, the analysis, conclusions, links to supporting source and concept pages.

---

## Workflows

### Ingest

When the human drops a new file in `raw/` and says to process it:

1. **Read** the source. For PDFs, read the text; note any charts or tables that are significant.
2. **Discuss** (optional but preferred): summarize the 3-5 most important takeaways and ask if there's anything specific to emphasize before filing.
3. **Update or create** concept, instrument, strategy, and event pages touched by the source. A single source may update 5–15 wiki pages. For each:
   - Add new information under the relevant section.
   - Note if the source contradicts or refines an existing claim (use a `> [!note]` callout to flag the update inline).
   - Add the source file path to the page's `sources:` frontmatter list.
4. **Update** `wiki/index.md` — add any newly created wiki pages (concepts, instruments, strategies, events, synthesis only — no source entries).
5. **Update** `wiki/overview.md` — revise the high-level synthesis if the source materially changes the picture.
6. **Append** to `wiki/log.md` — format: `## [YYYY-MM-DD] ingest | <SourceSlug>`, followed by the full source summary (origin, summary, key claims, contradictions, gaps).

### Query

When the human asks a question:

1. Read `wiki/index.md` to identify relevant pages.
2. Read those pages in full.
3. Synthesize an answer with inline citations like `([[MomentumFactor]], [[CarryTrade]])`.
4. Ask whether the answer is worth filing as a synthesis page. If yes, write it to `wiki/synthesis/`.
5. Append to `wiki/log.md`: `## [YYYY-MM-DD] query | <brief description>`.

### Lint

When the human asks for a health check:

1. Scan all wiki pages for: orphan pages (no inbound links), stale claims flagged with `[!note]` that may need resolution, concepts mentioned without their own page, missing cross-references.
2. Report findings as a prioritized list.
3. Ask which issues to fix, then fix them.
4. Append to `wiki/log.md`: `## [YYYY-MM-DD] lint | <summary>`.

---

## index.md Format

Sources are listed in plain text (no wikilinks — source files don't exist as pages). All other sections use wikilinks.

```markdown
# Wiki Index
_Last updated: YYYY-MM-DD — N pages total_

## Sources ingested (N)
- SourceSlug — one-line description (YYYY-MM-DD)

## Concepts (N)
- [[ConceptName]] — one-line description

## Instruments (N)
...

## Strategies (N)
...

## Events (N)
...

## Synthesis (N)
...
```

## log.md Format

Append only. Each entry:

```markdown
## [YYYY-MM-DD] ingest | SourceSlug
- Source: raw/articles/foo.md
- Pages created: wiki/sources/Foo.md, wiki/concepts/Bar.md
- Pages updated: wiki/concepts/Baz.md, wiki/people/Alice.md
- Notes: <one sentence on anything notable>
```

---

## Wikilink Policy

**Concept-only graph.** Wikilinks `[[...]]` are reserved exclusively for `concept`, `instrument`, `strategy`, `event`, and `synthesis` pages. **Never wikilink to `source` or `people` pages from any other page.** Source and people pages are bookkeeping artifacts — linking to them would pollute the Obsidian graph with non-conceptual nodes.

Concretely:
- Source pages (`wiki/sources/`) may contain wikilinks pointing *outward* to concepts, but nothing should link *back* to them.
- People pages (`wiki/people/`) should generally not be created; author attribution goes in plain text. If created, nothing should link to them with `[[...]]`.
- In source pages, list authors in plain text (e.g., "Cornelis W. Oosterlee (CWI Amsterdam)"), not as `[[PersonName]]`.
- In the `wiki/index.md`, list source page names without wikilinks (plain text only).

Forward-looking wikilinks (links to pages that don't exist yet) are **intentional and desirable**. They appear as grey nodes in Obsidian's graph view and signal concepts worth eventually covering. The human reviews them and decides which to keep.

**Removing a wikilink = "not interested in this concept."** If the human removes a `[[LinkName]]` from a wiki page, do not reintroduce that link in future ingests. Treat its absence as a deliberate editorial choice, not an oversight to fix.

### `scarta` — the pruning command

When the human types `scarta [[A]] [[B]] ...`, do the following for each named link:

1. **Grep** all files in `wiki/` for occurrences of `[[A]]`.
2. **Replace** every occurrence with plain unlinked text (e.g., `[[ImpliedVolatility]]` → `implied volatility`). Do not delete the surrounding sentence.
3. **Report** which files were edited and how many occurrences were removed.
4. **Append** to `wiki/log.md`: `## [YYYY-MM-DD] scarta | [[A]], [[B]], ...`

The command accepts any number of links in one call: `scarta [[A]] [[B]] [[C]]`.

---

## Conventions

- **Cross-reference aggressively.** If you mention momentum, link `[[MomentumFactor]]`. If you mention 2008, link `[[2008FinancialCrisis]]`. The graph view is the payoff for consistent linking.
- **Distinguish claim strength.** Use language like "X argues that…", "the evidence suggests…", "consensus view is…", "disputed: some papers find…". Don't flatten nuance into flat assertions.
- **Flag contradictions explicitly.** If source B contradicts source A on a claim, note both views on the relevant concept page rather than silently overwriting.
- **Personal notes are first-person.** When ingesting files from `raw/notes/`, treat them as the human's own observations and label them clearly on the page (e.g., "Per author's trade journal, 2024-03-15:…").
- **Dates**: always YYYY-MM-DD. Today's date is available in the system context.
- **Don't fabricate.** If a source doesn't say something, don't infer it into the wiki. Flag gaps as gaps.

---

## Creating pages without a source

The human can ask to create wiki pages directly from LLM knowledge, without providing a raw source file. This is useful for foundational concepts (e.g., mathematical definitions, well-established theory) that don't need a specific citation.

When creating a page from LLM knowledge:
1. Set `sources: []` and add `origin: llm-knowledge` in the frontmatter.
2. Open the page body with this callout:
   `> [!info] Written from LLM knowledge — no source ingested. Update when a relevant source is added.`
3. Write the page content as normal.
4. Update `wiki/index.md` and append to `wiki/log.md`: `## [YYYY-MM-DD] create | [[PageName]] (llm-knowledge)`.

The "Don't fabricate" rule still applies: write only what is accurate and well-established. If something is contested or uncertain, flag it explicitly. Do not invent citations or attribute claims to sources that weren't ingested.

When a real source is later ingested that covers the same concept, update the page, add the source to `sources:`, and remove the `origin: llm-knowledge` field if the page is now fully sourced.

---

## Bootstrapping

On first use, create the directory structure and the following stub files:
- `wiki/index.md` (empty index, N=0)
- `wiki/log.md` (empty, with a single `## [today] init | wiki created` entry)
- `wiki/overview.md` (one-paragraph placeholder: "No sources ingested yet.")

Then wait for the human to drop the first source.
