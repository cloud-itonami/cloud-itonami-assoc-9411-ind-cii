# cloud-itonami-assoc-9411-ind-cii

Industry rule/history catalog for the **Confederation of Indian
Industry** (CII) — the SIXTH entry aligned to **ISIC 9411** (activities
of business, employers, and professional membership organizations),
alongside
[`-9411-sau-fsc`](https://github.com/cloud-itonami/cloud-itonami-assoc-9411-sau-fsc)
(Saudi Arabia),
[`-9411-aut-wko`](https://github.com/cloud-itonami/cloud-itonami-assoc-9411-aut-wko)
(Austria),
[`-9411-irl-ibec`](https://github.com/cloud-itonami/cloud-itonami-assoc-9411-irl-ibec)
(Ireland),
[`-9411-nzl-businessnz`](https://github.com/cloud-itonami/cloud-itonami-assoc-9411-nzl-businessnz)
(New Zealand), and
[`-9411-cze-spcr`](https://github.com/cloud-itonami/cloud-itonami-assoc-9411-cze-spcr)
(Czech Republic). Part of the
[`cloud-itonami`](https://github.com/cloud-itonami) compliance-fact
family (ADR-2607141700, `cloud-itonami-compliance-fact-federation`,
in `com-junkawasaki/root`).

## Sourcing note

This repo fills India's previously-open association-axis gap. India
now has real, individually verified facts across all three axes:
country
([`cloud-itonami-iso3166-ind`](https://github.com/cloud-itonami/cloud-itonami-iso3166-ind)),
municipality
([`cloud-itonami-municipality-ind-new-delhi`](https://github.com/cloud-itonami/cloud-itonami-municipality-ind-new-delhi)),
and association (this repo).

Both entries here were directly WebFetch-verified against `cii.in`'s
own official "History" page
(`https://www.cii.in/about_us_History.aspx?gid=A`), which renders
successfully and states the founding lineage directly — no fallback
to Wikipedia was needed for this source. The 1895 founding date is
independently corroborated by Wikidata (Q842084)'s own "inception"
statement.

## Scope

A **read-only reference/archive** catalog — not an Advisor⊣Governor
actuation actor. It proposes or executes nothing on CII's behalf.

Coverage is reported honestly (see `association.facts/coverage`): an
association not in `catalog` has **no spec-basis**, full stop — never
fabricate one.

## Data

- `src/association/facts.kotoba` — the catalog (`association.facts`),
  source of truth.
- `schema/association-rule.edn` — DataScript schema.
- `data/datascript-tx.edn` — derived DataScript tx-data (query this
  alongside other `cloud-itonami`/`etzhayyim` compliance-fact sources via
  `com-junkawasaki/root`'s `scripts/compliance-fact-query.cljs`).
- `src/association_facts.kotoba` — the same catalog generated from the
  data file, compilable with `amu` to JS, wasm and both native ISAs.

[`docs/operator-quickstart.md`](docs/operator-quickstart.md) shows how to
read the data file, compile and query the port, check that the two
agree, and check a citation. Scripted fetches of `cii.in` get an Imperva
challenge page (HTTP 200), so a 200 is not proof the page was read.

Both entries directly WebFetch-verified against `cii.in`'s own
official History page: the 1895 founding of the Engineering and Iron
Trades Association (CII's earliest predecessor, by 5 engineering
firms in the Bengal Chamber of Commerce and Industry), and the 1
January 1992 renaming from Confederation of Engineering Industry to
Confederation of Indian Industry (CII) amid economic liberalisation.

## License

AGPL-3.0-or-later (matches the `cloud-itonami-iso3166-*` /
`-municipality-*` / `-assoc-*` / `-lei-*` convention). Policy text
itself remains CII's; this repo stores only citation metadata
(id/title/url/dates), not full text.
