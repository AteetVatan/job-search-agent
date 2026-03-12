# Job Search Agent

An AI-powered job search automation system that discovers jobs and applies via LinkedIn Easy Apply. **This is not a traditional software application** -- it is a structured set of prompts, configurations, and instruction files designed to work with any AI agent that has browser automation capabilities (such as Gemini CLI, Claude with computer use, or similar agentic AI tools).

No code to run. No dependencies to install. Just configure, paste the orchestrator prompt, and let the agent work.

## What It Does

- **Job Discovery** -- Searches Google Jobs, Indeed, Glassdoor, and Wellfound for relevant positions, filters by role/location/language, and presents ranked results
- **LinkedIn Easy Apply** -- Automatically fills and submits Easy Apply forms with your CV, cover letters, and form answers
- **Cover Letter Generation** -- 5 structurally different variants, strictly grounded to your CV (zero hallucination)
- **Anti-Detection** -- Random delays (30s-5min), dwell time, scroll variance, pagination randomization, CAPTCHA detection
- **Humanized Writing** -- All generated text passes AI detection by following strict voice, structure, and vocabulary rules

## Quick Start

### 1. Clone and Configure

```bash
git clone https://github.com/your-username/job-search-agent.git
cd job-search-agent
cp config.private.yaml.example config.private.yaml
```

### 2. Fill In Your Private Data

Edit `config.private.yaml` with your personal information:
- Name, email, phone, LinkedIn URL, GitHub URL
- Years of experience per technology
- Standard form answers (visa status, salary, availability, etc.)

### 3. Add Your CV

```bash
# Place your CV PDF in the documents/cv/ directory
cp /path/to/Your-CV.pdf documents/cv/cv.pdf
```

### 4. Write Your CV Source of Truth

Replace the template content in `cv-source-of-truth.md` with your full CV in plain text. This is the ONLY source the agent uses for generating cover letters and free-text answers -- it will never invent facts not found here.

### 5. Fill In Your Applicant Profile

Replace the placeholder values in `applicant-profile.md` with your personal info, work history, and skills.

### 6. Start the Agent

Open your AI agent tool and paste the entire contents of `orchestrator-prompt.md` as your first message. The agent will:
1. Read both config files
2. Load all skills and data files
3. Ask which mode to run
4. Begin the session

See [setup.md](setup.md) for the detailed setup guide and [how_to_run.md](how_to_run.md) for usage instructions.

## Project Structure

```
job-search-agent/
  config.yaml                  # Generic settings (checked in)
  config.private.yaml          # YOUR private data (gitignored)
  config.private.yaml.example  # Template -- copy this to get started
  orchestrator-prompt.md       # Paste this into your AI agent to start
  cv-source-of-truth.md        # Your CV content for text generation
  applicant-profile.md         # Quick-reference for form auto-fill
  documents/
    cv/                        # Your CV PDF (gitignored)
    certificates/              # Your certificates (gitignored)
    cover-letters/             # Agent-generated cover letters (gitignored)
  skills/
    anti-detection/            # Human-like browser behavior rules
    linkedin-easy-apply/       # Form handling procedures
    humanized-writing/         # AI-detection-proof text generation
    cover-letter-variants/     # 5 structurally different letter templates
    job-web-search/            # Multi-platform job discovery
  workflows/
    daily-apply-run.md         # Full LinkedIn Easy Apply session
    job-search-run.md          # Web job discovery session
    error-recovery.md          # CAPTCHA and error handling
```

## Configuration

The project splits configuration into two files to keep private data out of version control:

| File | What It Contains | Checked In? |
|---|---|---|
| `config.yaml` | Target roles, locations, platforms, timing, text generation rules | Yes |
| `config.private.yaml` | Name, email, phone, skills years, form answers, document paths | **No** (gitignored) |
| `cv-source-of-truth.md` | Your full CV for text generation grounding | Template checked in |
| `applicant-profile.md` | Quick-reference table for form auto-fill | Template checked in |

## Documentation

| Doc | What It Covers |
|---|---|
| [setup.md](setup.md) | Step-by-step first-time setup (8 steps) |
| [how_to_run.md](how_to_run.md) | Running modes, session behavior, troubleshooting |
| [how_this_works.md](how_this_works.md) | Architecture, config split, design decisions |

## Compatible AI Agents

This system works with any AI agent that can:
- Read local files
- Open and control a web browser (click, type, scroll, navigate)
- Follow structured instructions

Tested with:
- Gemini CLI / Antigravity (with browser subagent)
- Claude with computer use

## License

MIT
