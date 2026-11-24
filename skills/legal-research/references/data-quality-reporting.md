# Data Quality Reporting

> load: on-demand

Every report filed with `report_source_issue` is read by an automated reviewer.
It re-runs what you describe against Legal Data Hunter and files the problem
with the team that owns it. A report it can reproduce gets fixed, often within
days. A vague report cannot be checked and is set aside. **One precise report
is worth more than ten vague ones.**

## When to file

File a report when you have **checked** a concrete problem:

- **A specific document is missing.** You know it exists (a named act, article
  or decision) and neither `resolve_reference` nor `search` finds it.
- **Search misses documents it should find**, or returns clearly wrong ones
  (mis-indexed source, wrong court, legislation returned as case law).
- **Document text is outdated, cut off or garbled.** An old version is served
  as current, the text stops mid-way, or it is in the wrong language.
- **A document URL is broken** (404, or it opens a different document).
- **Wrong metadata** (date, court, document type) on a specific document.
- **An official source is not covered at all**: a court, gazette or
  legislature that `discover_sources` does not list.

Do **not** file:

- Unofficial material: academic commentary, practitioner articles, textbooks,
  benchmark or exam datasets. Legal Data Hunter covers official legislation,
  case law and official doctrine only.
- A topic search that is merely thin, without a specific document you expected.
- Anything you have not checked with the steps below.

Suggestions about the tools themselves (a confusing parameter, a missing
field) are welcome but should be rare: at most one per task, with
`issue_type="other"`.

## Before you file

1. **Retry once properly.** For a citation, call `resolve_reference` again with
   `hint_country` and `hint_type`. For a search, use the source's language and
   the filters from `get_filters`.
2. **Cross-check with the other tool.** If `resolve_reference` failed, try
   `search` for the same document, and the other way round. Say in the report
   whether the other tool found it: this tells the team whether the document is
   missing from our data or only the lookup is broken.
3. **Find the exact source ID** with `discover_sources` (e.g. `"FR/LegifranceCodes"`,
   `"DE/BVerfG"`). Use a bare country code (`"FR"`) only when no source fits,
   such as a source we do not have at all.

## How to file

- **One problem per report.** If you hit two (say, a missing decision and a
  broken URL), file two reports.
- `source`: the exact source ID from `discover_sources`.
- `issue_type`: `"unavailable"` (missing document or source), `"indexing"`
  (search finds the wrong thing or misses it), `"invalid_url"`, `"data_quality"`
  (outdated, truncated or garbled text; wrong metadata), `"other"`.
- `description`: use this template. Keep every line short and factual.

```
Tool call: <the exact call, with all parameters>
Expected: <the specific document: title, number / ECLI / CELEX, date, and why it should be there>
Got: <zero results / the ids actually returned / the error message>
Also tried: <the retry and the cross-check above, and what each returned>
Official link: <where the document exists on the official site, if you know it>
```

**Good:**

```
Tool call: resolve_reference("Article D1152-1 Code de la santé publique", hint_country="FR", hint_type="legislation")
Expected: Article D1152-1 of the Code de la santé publique, created by décret n° 2026-1043 of 12 Nov 2026, in force since 15 Nov 2026.
Got: resolved=false, match_type=not_found.
Also tried: search("D1152-1 Code de la santé publique", source=["FR/LegifranceCodes"]) returned only older D1151-* articles.
Official link: the article's page on legifrance.gouv.fr (copy the real URL; never build one)
```

**Bad:** `"Some health code articles are missing."`

For **outdated text**, add the version you were served (its date or a short
quote of the old wording) and the date the current version took effect.

**Never include client or matter details**: names of people or companies
you act for, facts of a case, internal references. Describe only the legal
source and the tool behaviour. Reports containing personal data are not
reviewed.

## How to tell the user

When a gap affects your answer, say so plainly:

> **Data gap noted**: [source] does not have [document]. I've reported it to
> Legal Data Hunter. In the meantime, [what you did instead, e.g. "I verified
> the text on Légifrance directly"].

Never silently skip a jurisdiction because of missing data. Flag the gap,
report it if it meets the bar above, and explain what you did instead.
