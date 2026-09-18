# Example LLM Prompts

A few starting points — copy, modify, and build your own style. The point isn't to use them verbatim; it's to see what good prompts look like. Notes on *why* follow each.

---

## Choosing the scope of your prompts

One of the first things to figure out with an agent is *how much to ask for at once*. You can hand it anything from a tiny piece to a whole open-ended question, and part of learning the tool is experimenting to see which scope fits you and the task. Roughly, from smallest to largest:

1. **A single function** — one small, reusable piece of code.
   *"Write a function that takes two lat/lng points and returns the distance between them in meters."*
2. **A single script** — one runnable file that does one job end to end.
   *"Write a script that loads both CSVs and prints summary stats for each."*
3. **A specific analysis** — a multi-step analytical task where the agent plans and runs the steps.
   *"Join each 311 complaint to its nearest box and show me the distribution of distances, with a plot."*
4. **An open-ended goal** — you hand over the question itself and let the agent decide the approach.
   *"Figure out whether the smart rat boxes appear to reduce rodent complaints, and take it as far as the data honestly allow."*

Smaller scopes give you more control and make each piece easy to check yourself, but mean more back-and-forth. Larger scopes are faster and require less typing, but the agent makes more decisions you don't see (which grid size? which rows dropped?) — so you have to scrutinize the result harder and ask it to show its work. **Try all of these at different points today** — the same question can be attacked at any scope, and seeing how the agent behaves at each is one of the best ways to learn what it can and can't do.

---

## Three ways to talk to the agent: execute, plan, critique

Beyond *how much* to ask for (above), there's *what kind* of thing you're asking the agent to do. Three modes, and knowing when to use each matters:

- **Execute** — *"Compute the correlation between boxes and complaints per neighborhood."* Fastest, but the agent makes choices you don't see (which grid size? which rows dropped?) and generally **won't flag problems you didn't ask about** — it does what you said.
- **Plan** — *"How would you approach measuring whether the boxes work? Draft a plan before writing code."* The agent lays out steps, assumptions, and data needs — and this is where it tends to **surface caveats** (selection bias, missing fields, confounders) it won't volunteer when you just ask it to compute.
- **Critique** — *"Here's my plan / here's the result — pressure-test it. What's wrong, what did I assume, what would a skeptical reviewer say?"* Asking the agent to argue *against* the work is the single most useful move if you can't check the code yourself: it's how a subtle flaw (a bad unit of analysis, a confounded comparison) gets caught before you rely on the number.

**Rule of thumb:** before you trust a result, ask the agent to *plan* or *critique* it — not just *compute* it. A clean-looking number that came straight from "execute" has had no one, including the agent, check whether the approach was sound.

---

## Prompt: First data look

```
I'm working with two CSV files:
- data/raw/smart_rat_boxes.csv: one row per smart rat trap device deployed by the City of Cambridge.
  Key columns: serial_number, device_name, address, latitude, longitude, coordinates,
  active_site (bool), device_type (Box or Pipe), total_since_install, prior_to_2023,
  total_2023, total_2024, total_2025, total_2026, participatory_budgeting (Yes/No)
- data/raw/rodent_311.csv: one row per citizen rodent sighting complaint submitted via 311.
  Key columns: ticket_id, issue_type, ticket_status, issue_description,
  ticket_created_date_time, address, lat, lng

Load both files. Print the shape, dtypes, missing value counts, and 3 sample rows for each.
Flag anything that looks like a data quality issue I should know about before doing analysis.
```

**Why this works:** Providing column names upfront means the agent doesn't guess what it's looking at. Asking for missing values and data-quality flags proactively prevents nasty surprises mid-analysis. It also gives it a real chance to catch this dataset's biggest gotcha — **the two files share no key**, so any link between them is one you construct from location and date, and `serial_number` repeats across redeployments (512 rows, 428 serials) — before it bites you in a join. (Point it at the files — don't paste rows in; pasting is where stale or mistyped values sneak in.)

---

## Prompt: Use the agent as a critic

```
Here's my draft analysis plan for the Cambridge rodent data. Review it and tell me:
1. Are there any steps in the wrong order?
2. What data-cleaning steps am I missing?
3. What assumptions am I making that I should state explicitly?
4. Is there a simpler way to answer my core question?

[PASTE YOUR DRAFT PLAN HERE]
```

**Why this works:** Using the agent as a critic rather than a writer often produces more useful feedback. Specific questions prevent a generic "looks good!" response.

---

## Prompt: When you're stuck

```
I'm stuck on [DESCRIBE THE PROBLEM]. Here's what I've tried: [LIST ATTEMPTS].
Here's the error / unexpected output: [PASTE IT].

Before giving me code, ask me two questions to make sure you understand what I'm
actually trying to do. I might be approaching this the wrong way.
```

**Why this works:** Asking the agent to ask *you* questions before answering slows the interaction down and often reveals you were trying to solve the wrong problem.

---

## Anti-Patterns to Avoid

- **Too vague:** *"Analyze the rat data"* — the agent will make choices you didn't intend.
- **Accepting the first answer:** ask the agent to critique its own plan before you run it, and to *show you the underlying rows/numbers* — that's verification, not "are you sure?" (which just gets you reassured).
- **Skipping sanity checks:** have it print intermediate results and re-check row counts at each join or aggregation — especially the join between the two files, which you built yourself and which nothing will validate for you.
- **Asking for everything at once:** break complex requests into steps — cleaning before analysis, analysis before visualization.
