# Guiding Questions & Entry Points

These questions get you thinking — they don't box you in. A good first analysis answers **one or two**, not all of them. They're ordered by difficulty: **descriptive → relational → inferential**. Climb as far as you like.

Any of them works on any of the three routes in the [README](../README.md#choose-how-you-want-to-approach) — the route decides what you *produce* (a statistic, a figure, a notebook), not which question you ask.

New to the data? Read [`../data/data_dictionary.md`](../data/data_dictionary.md) first.

---

## Descriptive (a good place to start)

- What's the distribution of catches across devices — are a few "hot" locations responsible for most of them, or is it spread evenly?
- Has the volume of 311 rodent complaints changed over time, and is there a seasonal pattern?

## Relational (join or compare both datasets)

- In areas with more active smart boxes, are there fewer 311 complaints than in areas with fewer boxes?
- Are there locations with many complaints but no nearby boxes — possible gaps in the program?

## Inferential (possibly subjective?)

- **Does the smart rat box program actually reduce rat sightings?** This is the core policy question. What would you need to control for, and what's a plausible comparison group?
- **How should the city evaluate the smart rat box program?** Maybe the program isn't about killing rats. How else can it be deemed useful? Or was it a waste of money?

---

## The reasoning trap to acknowledge

A common mistake is to conclude that *placing boxes where rats are caught* means *the boxes are working*. Watch for:

- **Selection bias:** boxes are placed where rats are already a known problem.
- **Temporal confounding:** other things change over time (construction, trash policy, a pandemic).
- **Reporting bias:** 311 complaints reflect resident awareness and willingness to report, not just rat activity.

Your plan doesn't have to solve these — but it should acknowledge them.

---

## Other data worth knowing about (not shipped here)

If you get stuck — especially on anything spatial — you'll likely want extra data: **Cambridge neighborhood polygons** (`rffn-qbt6`) to aggregate by area, or population / building permits / restaurant inspections as confounders. All of these live on the same [Cambridge Open Data Portal](https://data.cambridgema.gov/) as the two shipped files. Ask your LLM what would help and where to find it — and if a question needs data you don't have, note that as a limitation rather than improvising.
