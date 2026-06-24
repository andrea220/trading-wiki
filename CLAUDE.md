# Trading Wiki — Schema & Operating Instructions

This is a personal knowledge base about **institutional derivatives trading**, maintained by an LLM. You (the LLM) own the `wiki/` directory entirely. You read from `raw/` but never modify it. The human curates sources and asks questions; you do the summarizing, cross-referencing, filing, and bookkeeping.

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
    ├── log.md           ← append-only chronological record of all operations
    ├── overview.md      ← high-level synthesis of the current state of knowledge
    ├── concepts/        ← market concepts, pricing theory, risk measures, phenomena
    │   └── Concepts.md  ← hub page: links to all concept pages (graph cluster center)
    ├── instruments/     ← specific derivative products and underlying markets
    │   └── Instruments.md
    ├── models/          ← parametric pricing models and numerical methods
    │   └── Models.md
    ├── strategies/      ← trading strategies and approaches
    │   └── Strategies.md
    ├── regimes/         ← market regimes, vol crises, structural shifts relevant to derivatives
    │   └── Regimes.md
    └── synthesis/       ← comparison tables, analyses, answers to complex queries
        └── Synthesis.md
```

Each category folder contains one **hub page** (`Concepts.md`, `Models.md`, etc.) with `type: hub`. Hub pages link to every page in the category, creating a star-topology cluster in Obsidian's graph view. Update the relevant hub page whenever a new page is created in that category.

Create any missing directories as needed. Never delete raw source files.

---

## Page Format

Every wiki page starts with YAML frontmatter:

```yaml
---
type: concept | instrument | model | strategy | regime | synthesis | hub
tags: [tag1, tag2]
sources: [raw/articles/foo.md, raw/papers/bar.pdf]   # omit on hub pages
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

### `concept` — market concepts, pricing theory, risk measures
Lives in `wiki/concepts/`. Examples: `ImpliedVolatility.md`, `VolatilitySurface.md`, `OptionGreeks.md`, `RiskNeutralMeasure.md`, `ItoLemma.md`.
Captures: definition, intuition, mathematical statement where relevant, how it interacts with other concepts, practitioner debate, edge cases. Includes stochastic calculus foundations (Itô, BM, Q-measure) that underpin models but are not models themselves.

### `instrument` — specific derivative products and underlying markets
Lives in `wiki/instruments/`. Examples: `VarianceSwap.md`, `AutocallableCertificate.md`, `EuroStoxx50.md`, `VIX.md`, `ForwardStartOption.md`.
Captures: payoff structure, pricing considerations, key risks and Greeks sensitivities, typical use cases, relationship to models and strategies.

### `model` — parametric pricing models and numerical methods
Lives in `wiki/models/`. Examples: `HestonModel.md`, `LocalVolatilityModel.md`, `BlackScholesModel.md`, `MonteCarloSimulation.md`, `COSMethod.md`.
Captures: model specification (SDE or equation), calibration approach, closed-form results if any, known strengths and limitations, relationship to other models, implementation notes.

### `strategy` — trading approaches
Lives in `wiki/strategies/`. Examples: `DispersionTrading.md`, `CallOverwriting.md`, `ImpliedRepoTrading.md`, `VolatilityArbitrage.md`.
Captures: core logic, typical instruments, entry/exit rules as described in sources, known edge, known risks, evidence for/against, who uses it.

### `regime` — market regimes, vol crises, structural shifts
Lives in `wiki/regimes/`. Examples: `Volmageddon2018.md`, `Covid2020VolCrash.md`, `2022RatesRepricing.md`.
Focuses on events directly relevant to derivatives pricing and vol dynamics: what happened to the vol surface, how models behaved, what strategies worked or failed. Not general economic history.

### `synthesis` — analyses and complex answers
Lives in `wiki/synthesis/`. Created when a query produces a valuable answer worth preserving. Examples: `LVvsSLVForAutocalls.md`, `VolSurfaceDynamicsComparison.md`.
Captures: the question it answers, the analysis, conclusions, links to supporting concept, model, and instrument pages.

---

## Workflows

### Ingest

When the human drops a new file in `raw/` and says to process it:

1. **Read** the source. For PDFs, read the text; note any charts or tables that are significant.
2. **Discuss** (optional but preferred): summarize the 3-5 most important takeaways and ask if there's anything specific to emphasize before filing.
3. **Update or create** concept, instrument, model, strategy, and regime pages touched by the source. A single source may update 5–15 wiki pages. For each:
   - Add new information under the relevant section.
   - Note if the source contradicts or refines an existing claim (use a `> [!note]` callout to flag the update inline).
   - Add the source file path to the page's `sources:` frontmatter list.
4. **Update hub pages** — for each newly created page, add a wikilink to the relevant category hub (`Concepts.md`, `Models.md`, `Instruments.md`, `Strategies.md`, `Regimes.md`, `Synthesis.md`).
5. **Update** `wiki/overview.md` — revise the high-level synthesis if the source materially changes the picture.
6. **Append** to `wiki/log.md` — format: `## [YYYY-MM-DD] ingest | <SourceSlug>`, followed by the full source summary (origin, summary, key claims, contradictions, gaps).

### Query

When the human asks a question:

1. Read the relevant hub pages (`wiki/concepts/Concepts.md`, `wiki/models/Models.md`, etc.) to identify candidate pages.
2. Read those pages in full.
3. Synthesize an answer with inline citations like `([[HestonModel]], [[VolatilitySurface]])`.
4. Ask whether the answer is worth filing as a synthesis page. If yes, write it to `wiki/synthesis/` and add a link in `wiki/synthesis/Synthesis.md`.
5. Append to `wiki/log.md`: `## [YYYY-MM-DD] query | <brief description>`.

### Lint

When the human asks for a health check:

1. Scan all wiki pages for: orphan pages (no inbound links), stale claims flagged with `[!note]` that may need resolution, concepts mentioned without their own page, missing cross-references.
2. Report findings as a prioritized list.
3. Ask which issues to fix, then fix them.
4. Append to `wiki/log.md`: `## [YYYY-MM-DD] lint | <summary>`.

---

## Hub Page Format

Each category hub lives at `wiki/<category>/<Category>.md` with `type: hub`. It links to every page in the category, organized by sub-theme. Update it whenever a page is created or deleted.

```markdown
---
type: hub
tags: [hub]
created: YYYY-MM-DD
updated: YYYY-MM-DD
---

# <Category>

One-line description of what belongs here.

## Sub-theme A
- [[PageName]]
- [[PageName]]

## Sub-theme B
- [[PageName]]
```

Hub pages must **not** link to pages in other categories — cross-category connections belong on the knowledge pages themselves, not on hub pages. This keeps each category's graph cluster clean.

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

**Knowledge-only graph.** Wikilinks `[[...]]` are reserved exclusively for `concept`, `instrument`, `model`, `strategy`, `regime`, and `synthesis` pages. **Never wikilink to people or source tracking entries.** Author attribution and source references are bookkeeping — linking to them would pollute the Obsidian graph with non-knowledge nodes.

Concretely:
- List authors in plain text (e.g., "Cornelis W. Oosterlee (CWI Amsterdam)"), never as `[[PersonName]]`.
- In the `wiki/index.md`, list source slugs without wikilinks (plain text only).

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
4. Add a link to the relevant hub page and append to `wiki/log.md`: `## [YYYY-MM-DD] create | [[PageName]] (llm-knowledge)`.

The "Don't fabricate" rule still applies: write only what is accurate and well-established. If something is contested or uncertain, flag it explicitly. Do not invent citations or attribute claims to sources that weren't ingested.

When a real source is later ingested that covers the same concept, update the page, add the source to `sources:`, and remove the `origin: llm-knowledge` field if the page is now fully sourced.

---

## Bootstrapping

On first use, create the directory structure and the following stub files:
- `wiki/log.md` (with a single `## [today] init | wiki created` entry)
- `wiki/overview.md` (one-paragraph placeholder: "No sources ingested yet.")
- One empty hub page per category: `wiki/concepts/Concepts.md`, `wiki/instruments/Instruments.md`, `wiki/models/Models.md`, `wiki/strategies/Strategies.md`, `wiki/regimes/Regimes.md`, `wiki/synthesis/Synthesis.md`

Directories to create: `wiki/{concepts,instruments,models,strategies,regimes,synthesis}`.

Then wait for the human to drop the first source.
