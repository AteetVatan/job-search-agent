# How to Run

## Quick Start [3 Steps]

### 1. Edit Configuration

Open `config.yaml` and verify:
- Your LinkedIn search URL is current [update `platforms.linkedin.search_url`]
- Platform toggles are set [LinkedIn on? Web search on?]

Open `config.private.yaml` and verify:
- Your applicant info is correct
- Document paths point to your CV and certificates

### 2. Start an AI Agent Session

Open your AI agent tool [Antigravity, Gemini CLI, or similar] and start a new conversation.

### 3. Paste the Orchestrator Prompt

Copy the ENTIRE contents of `orchestrator-prompt.md` and paste it as your first message.

The agent will:
1. Read your config files [both `config.yaml` and `config.private.yaml`]
2. Load all Skills and data files
3. Ask which mode to run [if both platforms are enabled]
4. Begin the session

---

## Running Modes

### LinkedIn Easy Apply Only

Set in `config.yaml`:
```yaml
platforms:
  linkedin:
    enabled: true
  web_search:
    enabled: false
```

The agent will open LinkedIn, browse jobs, and auto-apply using Easy Apply. Random delays of 30s to 5 minutes between each application.

### Web Job Search Only

Set in `config.yaml`:
```yaml
platforms:
  linkedin:
    enabled: false
  web_search:
    enabled: true
```

The agent will search Google Jobs and Wellfound. It presents a ranked list of discovered jobs. It does NOT auto-apply to external sites -- you choose what to pursue.

### Both [Recommended]

Set both to `true`. The recommended flow:
1. Agent runs web search discovery first
2. Presents findings
3. You review and approve
4. Agent runs LinkedIn Easy Apply session

---

## During a Session

### What to Watch For

- **CAPTCHA**: The agent will stop automatically. Solve it manually in the browser, then tell the agent to continue.
- **"Unusual activity" warning**: The agent will stop. Do NOT dismiss the warning. Wait a few hours before retrying.
- **Stuck forms**: The agent will close and skip. No action needed.
- **Session report**: At the end, the agent gives you a full table of applied and skipped jobs.

### How to Stop Early

Simply tell the agent: "Stop" or "Pause". It will finish the current job [if mid-application] and generate a partial report.

### How to Resume

Start a new session [Step 2 above]. The agent checks for "Applied" badges on LinkedIn, so it will not re-apply to jobs from previous sessions.

---

## Updating Between Runs

Before each new run:
1. Open `config.yaml`
2. Update `platforms.linkedin.search_url` with a fresh LinkedIn search URL [or change keywords/filters on LinkedIn and copy the new URL]
3. Optionally update `job_search.target_roles` or `job_search.locations` if your search criteria changed
4. Start a new session

---

## Troubleshooting

| Problem | Solution |
|---|---|
| Agent does not load skills | Make sure all SKILL.md files are in the correct paths under `skills/` |
| Agent does not find private config | Ensure `config.private.yaml` exists [copy from `config.private.yaml.example`] |
| Agent applies to wrong roles | Check `config.yaml` -> `job_search.exclude_roles` and `target_roles` |
| Cover letters sound generic | Edit the variant files in `skills/cover-letter-variants/variants/` |
| Agent gets flagged too fast | Increase `anti_detection.delay_between_apps_min_seconds` in config |
| Agent makes up facts | Verify `cv-source-of-truth.md` has the hard grounding rule header |
| Web search finds irrelevant jobs | Tighten `job_search.keywords` and `job_search.locations` in config |
