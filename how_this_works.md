# How This Project Works

## Overview

Job Search Agent is a prompt-based AI automation system that helps you discover and apply to jobs. It is NOT a traditional software application with code you run. Instead, it is a structured set of prompts, configurations, and instruction files designed to work with AI agents that have browser automation capabilities [such as Antigravity, Gemini CLI with browser tools, or similar agentic AI systems].

## Architecture

```
+----------------------------------+
|       ORCHESTRATOR PROMPT        |
|  [You paste this to start]       |
|                                  |
|  Reads config.yaml               |
|  Reads config.private.yaml       |
|  Loads Skills + Workflows        |
|  Decides which mode to run       |
+--------+------------------------+
         |
   +-----+-----+
   |            |
   v            v
+---------+  +----------+
|LINKEDIN |  | WEB      |
|APPLY    |  | SEARCH   |
|WORKFLOW |  | WORKFLOW  |
+----+----+  +----+-----+
     |            |
     v            v
+----------------------------------+
|            SKILLS                |
|                                  |
|  anti-detection    [delays]      |
|  linkedin-easy-apply [forms]     |
|  humanized-writing [text rules]  |
|  cover-letter-variants [5 tpl]   |
|  job-web-search    [discovery]   |
+----------------------------------+
         |
         v
+----------------------------------+
|          DATA LAYER              |
|                                  |
|  config.yaml          [generic]  |
|  config.private.yaml  [private]  |
|  cv-source-of-truth.md   [CV]   |
|  applicant-profile.md    [forms] |
+----------------------------------+
```

## Configuration Split

The project uses two config files:

- **`config.yaml`** -- Generic settings shared across all users: target roles, locations, platforms, anti-detection timing, text generation rules. This file IS checked into git.
- **`config.private.yaml`** -- Your personal data: name, email, phone, skills years, form answers, document paths. This file is NOT checked into git. Copy `config.private.yaml.example` to get started.

## How It Flows

### Mode 1: LinkedIn Easy Apply

1. You edit `config.yaml` with today's LinkedIn search URL
2. You paste `orchestrator-prompt.md` into an AI agent session
3. The agent reads both config files and loads all Skills
4. It opens LinkedIn in a browser, scrolls the job list
5. For each job: it checks skip conditions, dwells on the description [anti-detection], fills the Easy Apply form, submits
6. Between each application, it waits a random 30 seconds to 5 minutes
7. It continues through all pages until done or stopped
8. It generates a report of what was applied and what was skipped

### Mode 2: Web Job Search

1. The agent searches Google Jobs, Glassdoor, and Wellfound for your target roles and locations
2. It extracts job listings, filters out excluded roles, and scores relevance
3. It presents a ranked table of discovered jobs
4. You decide which to pursue: some may be LinkedIn Easy Apply [routed to Mode 1], others are external

### Anti-Detection

The system avoids bot detection through:
- **Random delays**: 30 seconds to 5 minutes between each application [configurable]
- **Dwell time**: scrolling through job descriptions for 8 to 45 seconds before acting
- **Scroll behavior**: variable speed, occasional scroll-back, pauses
- **Pagination randomization**: not always clicking the next sequential page
- **Warm-up**: browsing casually for 30 to 60 seconds before starting applications
- **CAPTCHA detection**: immediately stops if any security challenge appears

### Text Generation [No Hallucination]

All generated text [cover letters, free text answers, messages to recruiters] uses ONLY facts from `cv-source-of-truth.md`. The system will never invent company names, inflate metrics, or claim skills not in the CV. Five structurally different cover letter variants are rotated based on job type.

## Key Design Decisions

**Why prompts instead of code?**
Because the execution environment is an AI agent with a browser, not a traditional runtime. The "code" is the structured instructions the agent follows. This means zero dependencies, zero installation, and works with any agentic AI tool that has browser capabilities.

**Why two config files?**
So you can share the project on GitHub without exposing your personal data. `config.yaml` has generic settings. `config.private.yaml` has your private info and is gitignored.

**Why separate cover letter variants?**
Because sending the same cover letter to 100 companies is a spam signal. Five variants with different structures, tones, and emphasis areas look like a human who writes differently depending on the company.

**Why strict CV grounding?**
AI systems hallucinate. A single invented claim in a cover letter destroys credibility if a recruiter checks. The hard rule [every claim must trace to the CV file] prevents this.
