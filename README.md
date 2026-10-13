# UK marine fishing closures — an open dataset

**Every legally closed fishing area in UK and Crown Dependency waters, in one
place: the boundary, what the rule prohibits, and the instrument that creates
it.**

271 closures, assembled and classified by [CAUGHT](https://caught.org.uk).
Free to use, including commercially, with attribution.

---

## Why this exists

There is no single official list of where fishing is restricted in UK waters.
The rules are made by **fifteen different authorities** under their own
instruments, published in different formats and to different conventions, and
sea fisheries are devolved — so the answer changes at the border between
Scotland, England, Wales, Northern Ireland, the Isle of Man and Jersey.

Answering "is this area closed, to what, and under which law?" currently means
visiting fifteen sources and reconciling them by hand. This dataset is that
reconciliation, published because it is more useful to more people than it is to
one project.

It was assembled in order to **watch inside those closures from satellite and
vessel-tracking data** — that is what CAUGHT does — but the list itself stands
on its own.

## What's here

| File | Rows | What it is |
|---|---|---|
| `data/closures.csv` | 271 | One row per closure: name, authority, nation, what is prohibited, the instrument and its legislation link, area, and how many cases CAUGHT has recorded inside it. |
| `data/closures.geojson` | 440 features | The same fields with boundary geometry, ready for QGIS, Leaflet or any mapping tool. |
| `data/case-stats.csv` | daily | Aggregate counts of CAUGHT cases per day by status. No vessel is named. |

There are **more polygons than closures** because a single closure is often
several disjoint areas — a byelaw covering three bays is one rule and three
boundaries. Group on `name` for one row per closure; the CSV already does.

## Fields

| Column | Notes |
|---|---|
| `name` | The closure's name as published in the source dataset. |
| `slug`, `url` | Its page on caught.org.uk, where the rule is set out in full. |
| `nation` | Scotland / England / Wales / Northern Ireland / Isle of Man / Jersey. **Derived** — the source records the authority that made the rule, not the country it applies in. |
| `authority` | The body that made the rule. |
| `restriction_class` | **Our classification**, not the authority's wording. One of `no_take`, `demersal_ban`, `bottom_towed_ban`, `trawl_ban`, `dredge_ban`. See the caveat below. |
| `restriction_label`, `restriction_summary` | The source dataset's own description of the prohibition. |
| `legislation_name`, `legislation_type`, `legislation_date`, `legislation_link` | The instrument creating the closure. `legislation_link` may contain several URLs separated by `\|` where a rule spans articles. |
| `area_km2` | Area of the closure. |
| `cases_recorded` | How many CAUGHT cases fall inside it. A count only — see "What is deliberately not here". |

## Three caveats, and please read them

**1. The boundaries are simplified.** The geometry here is the version drawn on
the map, reduced so it renders quickly. It is accurate enough to show *where* a
closure is and **not accurate enough to decide whether a particular position
falls inside it**. For that, use the authority's own published boundary — the
`legislation_link` on each row leads to it. A closure boundary is a legal line
and should be treated as one.

**2. `restriction_class` is our reading, not the authority's wording.** It is
derived from the measure's summary text, and each class deliberately claims a
**subset** of what is really prohibited — it is built to under-claim rather than
over-claim, because the cost of the two errors is not symmetrical. Where we have
read the instrument itself, that reading is on the closure's page at
caught.org.uk, and **the instrument governs**: a dataset paraphrase can be
narrower or wider than the law it summarises.

**3. The distinction that matters most.** Nearly every closure here bans one
*kind* of fishing rather than fishing itself — usually gear towed across the
seabed. **Static gear (creels, pots, set nets) is normally still permitted**, so
a vessel fishing inside a bottom-towed gear ban is very often fishing entirely
lawfully. Only the ten `no_take` closures ban the activity outright. Any analysis
that treats "vessel inside closure" as "vessel breaking the law" will be wrong,
and wrong in a way that accuses real, identifiable people.

## What is deliberately not here

**There is no per-vessel data and no case-level export.** CAUGHT judges each case
on three independent factors — whether fishing happened there and then, what gear
the vessel actually uses, and what was prohibited there at that time — and the
weakest of the three decides. Most cases are examined and resolved.

A spreadsheet of vessel names would travel without any of that, could not be
corrected once copied, and could not be withdrawn. Cases are published one at a
time, each with its evidence and its status, at `caught.org.uk/case/<id>`.

If you are researching something that needs more than this, write and say what
the question is: contact@caught.org.uk.

## Licence

The closure boundaries and restriction summaries are public sector information
from **Seafish Kingfisher, NatureScot, JNCC and Natural England**, licensed under
the [Open Government Licence v3.0](https://www.nationalarchives.gov.uk/doc/open-government-licence/version/3/).

CAUGHT's own contributions — the assembled list, the nation mapping, the
restriction classification and the rule research — are released under
[CC BY 4.0](https://creativecommons.org/licenses/by/4.0/).

Use it freely, including commercially. Credit the source.

## Citation

```
CAUGHT (2026). UK marine fishing closures [Data set]. https://caught.org.uk/data
```

See `CITATION.cff` for a machine-readable version. If you publish something that
uses this, we would like to know — contact@caught.org.uk.

## Updating

The dataset is regenerated from the live service. The current copy is always at:

- <https://caught.org.uk/data/closures.csv>
- <https://caught.org.uk/data/closures.geojson>
- <https://caught.org.uk/data/case-stats.csv>

`cases_recorded` moves as satellite sweeps run; everything else changes only when
the underlying closure dataset is rebuilt.
