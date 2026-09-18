# Getting Started: Setting Up Claude Code

This workshop uses **Claude Code**, Anthropic's coding agent, through Harvard's Anthropic subscription. Everyone uses the same tool so we can help each other and share prompts. Get it working **before** the first session. If you are a Harvard affiliate, see the [instructions on how to set up Claude code here](https://github.com/harvardinformatics/llm-guides). If you are not a Harvard affiliate, feel free to use your own coding agent.

The data is already in `data/raw/` — there's nothing to download. To start with, skim [`../data/data_dictionary.md`](../data/data_dictionary.md): you'll write much better prompts if you already know what's in the files.

You will also need a language to work in. Claude Code writes the code and runs it — but it runs it **on your computer**, using software you installed. Work in **Python or R**, whichever you already know; nothing in this workshop depends on the choice. If you know you have neither, download a fresh R installation from **<https://cloud.r-project.org>**. R is a common coding language for data/statistical analysis. 

If you want to do graphing, you may want to install libraries/packages relating to graphing such as matplotlib for python or ggplot for R. **Always double check when the agents asks to download/install new packages**

---

# A short history of the rat boxes

There's a great [Cambridge Day](https://www.cambridgeday.com/2025/11/25/rats-resist-efforts-to-cull-cambridge-will-try-faster-scrap-removal/) article from 2025 about the rat boxes and their history. 

The program started back in May 2022 with *Boxes* and *Pipes* installed on the streets and in the sewer, respectively. Each time a rat enters a box, the smart device detects the movement and body heat and electrocutes them. The sewer pipes let the rats get washed out while the boxes fill up with rats. Each rat kill gets recorded with a timestamp. 

After a couple of years, it was found that the sewer pipes were not getting very many rats (only 335 in 2025 at the time the above article was written, compared with 1149 from boxes). So all the pipes were removed and now we only have boxes. This is why all the pipes are marked inactive in the dataset, but you can be the judge for whether that was the right call!

Spoiler alert for those curious, the rat boxes accomplish more on the identifying rat hotspots front than on the population control front. In the end, Cambridge officials have found that prevention/mititgation is more effective that culling. But let's analyze the data and look for interesting patterns anyway!

---

# Journaling your progress

Just like experimentalists/bench scientists, bioinformaticians and data scientists find it useful to keep a notebook of their work. This helps thems plan and keep track of their questions, analysis pathways, and files. As a pedagogical tool, it's also useful because when working with LLMs, it's important to take a step back frequently and evaluate whether the prompts you are using & responses you are getting back are actually propelling your forward or if you are getting stuck in a loop. It's also a way to deliberately slow down the prompt-response cycle to facilitate reflection and thus learning.

In this workshop, we will have everybody fill out a short journaling prompt **for every LLM session**. The format looks like this:

```
### [DATE & TIME]

**What I was trying to do:**


**Prompt(s) I used (copy-paste the actual prompt):**

**Model/Effort:**


**What I expected vs. what I got:**

**My next step:**

```

You can find the journal in this repo under `journal/my_journal.md`. Let's give it a whirl now! I'm going to ask it "Give me the number of catches per device type as a table."
