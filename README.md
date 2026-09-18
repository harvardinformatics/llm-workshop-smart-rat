# Cambridge Rodent Data — Workshop Project Folder

**GenAI for data analysis | Rat boxes track**

Welcome. This folder is your workspace. Over the workshop you'll use an LLM agent (Claude Code) to explore two real, public datasets about rodents in Cambridge, MA: where the city put its rat traps and what those traps caught, and where residents reported seeing rats. The goal of this workshop is to **get to know GenAI-assisted data analysis** — how to prompt, how to check work, and how to keep track of what you did. A secondary goal is to share what you've learned with everyone else in the group, because using AI can be silo-ing and we are trying to counter that!

You do **not** need to have worked with city data, maps, or public-health data before. The data is deliberately ordinary — two CSVs anyone can read — so that the focus can be on using the LLM to make cool things you otherwise wouldn't be able to.

This workshop is divided into two days:

Day 1 focuses on exploring the dataset on your own and getting a feel for how to work with the agent. The approximate agenda is as follows:

- **Tutorial/Warmup ~30 minutes:** I'll introduce the dataset, the background, and we will all go through the same warmup prompt together. This is a good time to ask questions about the data, how to work with the agent, and any tips for getting started. 
- **Individual work ~30 minutes:** Choose a question to explore, plan your analysis, and let the agent help you execute it. Journal entries along the way to keep track of what you did and what worked/didn't work.
- **Individual report out ~15 minutes:** Share your findings and reflections with the group. Focus on how you interacted with the LLM, what surprised you, what confused you, and what you thought went well. 
- **Snack break ~10-15 minutes:** Eat snacks, get coffee.
- **Group work ~45 minutes:** Real science doesn't happen in a vacuum and this is the part where we counter the anti-social AI-scientist self-reinforcing feedback loop. We'll split into groups of 2-3 and each group will dig a bit deeper together into the dataset. Have roles (e.g. who will prompt, who will do journaling, etc) and discuss what you find. The goal is to get a sense of how to work with the agent in a collaborative setting, and to see how different people approach the same dataset.
- **Group report out & general discussion ~30+ minutes:** Each group will share their findings and reflections with the larger group.

Day 2 focuses on approaching GenAI-assisted data analysis with more rigor and reproducibility. Agenda:

- **Lecture/discussion on best practices and pitfalls:** I'll give a short lecture on what we (Informatics group) have learned about working with GenAI in a research context. This is mostly based on "feels" rather than hard data, and things are always evolving, so let this also be a discussion where you can share your own experiences.
- **Group work on reproducible analysis:** We will do a fresh pull of the dataset and perform an analysis from scratch in such a way that a polished, reproducible report can be generated. 
    - We'll update the dataset to the current pull, and rerun anything. This will mimick getting new data and having to re-do your analysis, as a test of reproducibility
    - This would be a good time to try to incorporate another data source, such as Cambridge neighborhood polygons, restaurant data, etc. 
- **Snack break ~10-15 minutes:** Eat snacks, get coffee.
- **Group share out:** Each group will present about how they organized their work, what features of their report are reproducible, and what they learned. 

---

## The data in one paragraph

![smart-box](https://www.cambridgema.gov/-/media/Images/participatorybudgeting/pbcycles/pbcycle10/smartbox.png?h=383&w=350)

Cambridge, MA has a rat problem, and it has spent several years doing two things about it. First, it deployed a network of IoT "smart boxes" — traps on public property that report what they catch — and it publishes the catch counts per device. Second, like most cities, it takes rodent complaints from residents through its **311** system, and it publishes those too. The two datasets are the city's *intervention* and the public's *experience of the problem*, recorded independently. Put together, they invite an obvious question:

> Where are rats a problem in Cambridge, and is the city's intervention working?

That question has some nuance to it, and the data aren't particularly clean. But there are only two CSVs:

| Dataset | Records | Coverage | Update frequency |
|---|---|---|---|
| [Smart Rat Boxes](https://data.cambridgema.gov/Public-Works/Smart-Rat-Boxes/78bs-b5ig/about_data) | 512 deployments (428 devices) | catch totals 2023–2026 | weekly |
| [311 Rodent Sightings](https://data.cambridgema.gov/Public-Works/Commonwealth-Connect-Service-Requests-Rodent-Sight/uz4w-baz5/about_data) | 4,973 complaints | 2013–2026 (nothing in 2014–15) | daily |

Both files are already in `data/raw/` — a **frozen snapshot from July 2026**, deliberately not kept current. Provenance and column-level detail are in [`data/data_dictionary.md`](data/data_dictionary.md).

---

## Read Orientation Materials

[Data Dictionary](data/data_dictionary.md) <-- Start here, get to know the data

[Quick Start](orientation/02_quick_start.md) <-- then recreate your first figure

---

## Choose how you want to approach

There's more than one way in. You can start with an analysis that is familiar to you so that you can check the work of the LLM. Or you can try to do something unfamiliar and try to apply some critical thinking skills as to how you can be more of a "manager" of the LLM. Below are just some ideas. Feel free to mix and match/combine/ or pick something entirely different. 

### 📊 Route 1 — Statistics
**You want to practise reasoning about evidence with an agent.** Ask the agent to write statistical summaries and apply tests to the data. It'll spit out tables and p-values. But you need to be the one to understand what tests to perform. 

### 📈 Route 2 — Figures
**You want to practise visualization.** Get the agent to make the plots and spend your own effort on editing them — what's misleading, what's missing a denominator, what a reader would misread. To extend behind bar plots and such, you could even make a browser-based interactive map of all the 311 reports overlaid with the rat box activity over time. 

### 📓 Route 3 — Notebook
**You want to practise reporting out/reproducibility.** Build one document (Quarto, Jupyter, or R Markdown) that goes from `data/raw/` to conclusion and runs top to bottom on a machine that isn't yours. This is both a good format for exploratory data analysis and writing a report that someone else can recreate. Can incorporate stats, figures, etc. 

Questions to attach to any of these are in [`orientation/03_guiding_questions.md`](orientation/03_guiding_questions.md). The route decides what you produce, not what you ask.

---

## Folder structure

```
workshop/
├── README.md                          ← you are here
├── LICENSE
├── .claude/
│   ├── settings.json                  ← project settings for Claude Code
│   └── commands/
│       └── audit-files.md             ← a slash command to try on Day 2 (see 05_claude_organization.md)
├── orientation/
│   ├── 01_getting_started.md          ← set up Claude Code + journaling progress
│   ├── 02_quick_start.md              ← your first task — recreate the figure
│   ├── catches_vs_complaints.png      ← the figure you'll recreate
│   ├── 03_guiding_questions.md        ← (optional) questions to explore + the reasoning trap
│   ├── 04_example_prompts.md          ← (optional) copy-paste prompts to try with the agent
│   └── 05_claude_organization.md      ← keeping an agent-assisted project organized (Day 2)
├── data/
│   ├── data_dictionary.md             ← what every file is + gotchas (start here)
│   └── raw/
│       ├── smart_rat_boxes.csv        ← 512 device deployments + their catch counts
│       └── rodent_311.csv             ← 4,973 citizen rodent complaints
└── journal/
    └── my_journal.md                  ← your reflection log — edit throughout
```

---

## Key constraint to keep in mind

The smart box data reports catches per device, but **a count of zero does not mean no rats** — it may mean the device malfunctioned, was inactive, or was recently installed. The 311 data is citizen-reported, so it measures who noticed and bothered to file, not where rats are. And the two files **share no key at all**: any link between them is one you build yourself, out of location and date. Any analysis has to grapple with all three.

---

## Glossary

**311** — the general-purpose number and app US cities use for non-emergency service requests. A "311 complaint" here is a resident reporting a rodent sighting.

**Socrata** — the platform hosting Cambridge's open data portal. Each dataset has a short ID (`78bs-b5ig`, `uz4w-baz5`) that addresses it in both the web portal and the API. (Claude can help you write scripts to download data from Socrata!)

**Smart box** — an IoT rat trap on public property that reports what it catches. Deployed and serviced by Modern Pest Services under contract to the city.

**Box vs. Pipe** — the two device types. A `Box` sits above ground; a `Pipe` goes into a sewer. They're placed on different logic and catch at different rates. 

**Catch count** — the number of rats a device recorded, cumulatively (`total_since_install`) or per year. Often described as a "kill count" in the city's own materials.

**Active site** — the city's flag for whether a device is currently deployed. 280 of the 512 rows are marked active. What an *inactive* row means is less obvious than it looks, and the file doesn't say.

**Deployment vs. device** — one row is a *deployment*. Moving a device to a new location creates a second row with the same `serial_number`, which is why there are 512 rows but 428 serials. Which of the two is your unit of analysis is a decision you should make explicitly.

**Participatory budgeting** — Cambridge's program letting residents vote directly on how to spend part of the city budget. 168 devices were funded this way, which makes the flag a natural handle for questions about who gets the intervention.
