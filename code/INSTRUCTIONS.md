# Language File Creation — Instructions

You are creating language profile files in `files/`, one per language listed in the CSVs under `data/`. The canonical example is `files/American English (en-US).md` — read it once at the start of every session. Structure is defined in `code/TEMPLATE.md`; research guidance per section is in `code/RESEARCH-GUIDE.md`.

## Hard constraints

1. **Budget-limited sessions (Pro subscription).** Do NOT research an entire language before writing. Work section by section: research one section → write it to the file → save → move to the next. If the session ends mid-file, no work is lost and the next session resumes cleanly (see "Resuming").
2. **One agent only.** Never spawn subagents or parallel tasks. All research and writing happens in the main loop, sequentially.
3. **One language per session, maximum.** If a language is finished and budget clearly remains, you may start the skeleton of the next language, but never leave more than one file incomplete.

## The voice

These files are satirical in-universe encyclopedia entries: **real, researched data wearing a playful narrative costume.** Follow the en-US example:

- Tables and `key :: value` fields contain genuine facts (populations, dates, titles, statistics).
- Prose (intro paragraph, eras blurb, origins events) is written with wry, mock-mythological framing — e.g. American English is "constructed by rebellious pirates with ambitions for world domination."
- **Holocene dates everywhere**: Gregorian year + 10000. 1775 CE → `11775`. For BCE: 10000 − year, so 3000 BCE → `7000`. Only parenthetical years inside proper titles keep Gregorian form, e.g. `The Great Gatsby (1925)`, `Princeton University (1746)`.
- Ages in parentheses after people/entities in origins events: `Noah Webster (70yo)`, `Microsoft (29yo)`.
- `==highlight==` marks in origins for milestone concepts: `==written-form==`, `==signed-form==`, `==capital-city==`.
- The frontmatter `type` is an in-universe judgment (en-US, a living language, declares itself `constructed` because pirates built it). Default to the CSV's type (`living`/`dead`/`constructed`) unless your intro narrative earns a different claim.

## Where languages come from

The work queue is the union of:

- `data/living-languages.csv`
- `data/dead-languages.csv`
- `data/constructed-languages.csv`

More rows and possibly more CSVs will be added over time — always glob `data/*.csv` rather than hardcoding. The CSV columns `language`, `type`, `global-speakers`, `parent-language` are authoritative; the file you write must agree with them.

**Filename**: exactly the CSV `language` value plus `.md`, e.g. `files/British English (en-GB).md`, `files/Klingon (tlh).md`, `files/Dothraki.md` (no code in CSV → no code in filename). Exception: `/` is invalid in filenames — replace ` / ` with ` - `, e.g. `Yue Chinese / Cantonese (yue)` → `files/Yue Chinese - Cantonese (yue).md`. Inside the file (wikilinks, names), use the sanitized filename in link paths but the full CSV name in `spoken-name`/`aka` fields.

## Session workflow

Every session, in order:

1. Read `files/American English (en-US).md` (the reference), this file, and `code/TEMPLATE.md`.
2. **Find resumable work**: search `files/*.md` for the string `%% TODO`. If any file contains it, that file is your language for this session — skip to step 5.
3. Otherwise pick the next language: first row across `data/*.csv` (in the order living → dead → constructed, top to bottom) with no corresponding file in `files/`.
4. **Create the skeleton**: copy `code/TEMPLATE.md` into `files/<Language Name>.md`, substituting the language name into every wikilink path and filling frontmatter. Save immediately. Every section body starts as a `%% TODO: ... %%` placeholder.
5. **Fill sections one at a time**, in this order (cheapest research first):
   1. `written` — locale code, meaning/etymology, reader counts, scripts/typefaces
   2. `spoken` — speaker counts, locations, parent language, dialect table
   3. `signed` — signed form if any (see adaptation rules)
   4. `eras` — historical periodization blurb + table
   5. intro paragraph (write after you know the language's story)
   6. `ambassadors` — the 12 top-5 tables (the expensive part; each top-5 table is its own research+write step)
   7. `origins` — reverse-chronological event timeline (15–20 rows for major languages, 8–12 is fine for small ones)
   For each: do the research (see RESEARCH-GUIDE.md, respect its query budget), then a single Edit replacing that section's `%% TODO %%` with final content, then move on. Never batch multiple unwritten sections' research.
6. When no `%% TODO` remains in the file, change frontmatter `status: draft` → `status: review` and stop. Tell the user which language was completed and which is next in the queue.
7. If you sense the budget running low mid-file, finish the current section's Edit, then stop and report exactly which sections remain (they're self-evident from the `%% TODO` markers anyway).

## Resuming

The `%% TODO: section-name %%` Obsidian comments are the entire progress-tracking system — no separate progress file. A file is incomplete iff it contains `%% TODO`. Frontmatter `status` is `draft` while incomplete, `review` when done (the human flips it further from there). Never delete a TODO marker without writing real content in its place.

## Adaptation rules by language type

The template's six sections (spoken / written / signed / eras / ambassadors / origins) always exist, but their contents flex:

**Dead languages** (Latin, Sumerian, ...):
- `spoken`: `global-speakers` comes from the CSV (usually `0 (academic use only)` etc.). Dialect table → historical dialects/registers (e.g. Classical vs. Vulgar Latin). `speaker-locations` → where it was spoken at peak, plus current liturgical/academic strongholds.
- `written`: usually the strongest section — scripts are real (cuneiform, hieroglyphs). Typeface table → Unicode fonts that render the script (e.g. Noto Sans Cuneiform).
- `signed`: almost never exists. Write `signed-name :: none attested` and one dry in-universe line (e.g. "The dead sign only in dreams."). Keep the section header.
- `eras`: peak → decline → death → afterlife (liturgical/academic).
- `origins`: end the timeline at the language's death/decipherment; ==decipherment== is a highlight-worthy milestone.

**Constructed languages** (Esperanto, Klingon, ...):
- `spoken`: CSV speaker figure; locations → conventions, online communities, fan enclaves.
- `written`: script the creator designed (pIqaD, Tengwar) plus romanization; `Google translatable` / `Suno readable` are real checks — verify.
- `signed`: `none attested` unless one exists (rare).
- `eras`: creation → publication → community adoption → media boosts.
- `origins`: creator-centric; the creator's dictionary/grammar publications are milestone events.

**Ambassadors — adapt the categories.** Keep exactly 12 subsections, each `##### top-5 <category> (<role>)`, using all 12 roles once: ranger, rogue, cleric, druid, paladin, barbarian, wizard, warlock, monk, fighter, bard, sorcerer. For living languages, prefer the en-US set (cities, actors, fandoms, families, drinks, foods, schools, wars, books, games, songs, films). Where a category can't produce five real rows, swap it for one that can — suggested pools:

- Dead: texts, scholars, inscriptions/artifacts, ancient cities, rulers, deities, manuscripts, loanwords donated to living languages, museums holding its artifacts, wars.
- Constructed: works written/translated in it, creators & contributors, conventions/events, media appearances, fan communities, famous phrases, learning resources, songs recorded in it.

Match role to category flavor when possible (bard → songs, warlock → wars, monk → books/texts, wizard → schools/scholars). Every top-5 table needs a name column plus at least one researched stat column. Never invent statistics — if you can't source five rows for a category, pick a different category.

## Formatting rules (Obsidian-specific)

- Inline fields use Dataview syntax: `key :: value` with a blank line between fields.
- Section headers are `#### name` followed by a `---` line; ambassador subsections are `##### top-5 X (role)` with no `---`.
- Frontmatter is exactly: `consciousness: smg108f`, `template.name: language`, `template.version: "1.0"`, `type: <see voice rules>`, `status: draft`.
- Origins table is reverse-chronological (newest first). Eras table likewise.
- Numbers use compact notation matching en-US: `350M`, `500K`, `1.5K`, `$520B`.

## Accuracy

Real data only, from web research — the satire lives in the framing, never in the numbers. When sources disagree, use ranges like the en-US wars table (`50K to 100K`). Cite nothing in the file itself; it's an in-universe document.
