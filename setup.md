# Setup Guide

## Prerequisites

You need an AI agent tool with browser automation capabilities. Any of these work:
- **Antigravity** [Gemini CLI with browser subagent]
- **Claude** with computer use / browser tools
- **Any agentic AI system** that can read files, open a browser, click, type, scroll, and navigate web pages

You also need:
- A LinkedIn account [logged in via the browser the agent will use]
- Your CV as a PDF file
- 5 minutes to customize the config

## Step 1: Create Your Private Config

Copy the example file to create your private configuration:

```
cp config.private.yaml.example config.private.yaml
```

Open `config.private.yaml` and update ALL sections with your own details:

### Applicant Section
Replace all values with your own details:
```yaml
applicant:
  name: "Your Name"
  title: "Your Title"
  email: "your@email.com"
  phone: "+your phone"
  location: "Your City, Country"
  portfolio: "https://your-portfolio.com"
  linkedin: "https://www.linkedin.com/in/your-profile/"
  github: "https://github.com/your-username"
  citizenship: "Your citizenship and work rights"
  visa_required: false  # or true
  availability: "Immediately"  # or your start date
  salary_expectation: "75000 EUR"  # your target
```

### Skills and Years
Update the `skills_years` section with your actual years of experience per technology.

### Standard Answers
Update the `standard_answers` section with your responses to common form questions.

## Step 2: Edit config.yaml [Optional]

Open `config.yaml` to customize generic settings:

### Job Search Section
Set your target roles, exclusions, locations, and keywords:
```yaml
job_search:
  target_roles:
    - "Your Target Role 1"
    - "Your Target Role 2"
  exclude_roles:
    - "Roles You Do Not Want"
  locations:
    - "Your Target Locations"
  keywords:
    - "Your Search Keywords"
```

## Step 3: Update cv-source-of-truth.md

Replace the contents of `cv-source-of-truth.md` with YOUR CV content. Keep the grounding rule header at the top:

> **HARD GROUNDING RULE**: All generated text MUST use ONLY facts from this document.

Then paste your full CV below it. Use plain text, no special characters.

## Step 4: Update applicant-profile.md

Replace the placeholder values in `applicant-profile.md` with your own personal info, work experience, skills, and standard form answers.

## Step 5: Customize Cover Letter Variants

Edit the 5 files in `skills/cover-letter-variants/variants/`:
- `variant_direct.md` -- short and confident
- `variant_technical.md` -- technical depth
- `variant_startup.md` -- founder mentality
- `variant_leadership.md` -- team and strategy
- `variant_project.md` -- project highlight

The templates use placeholder references. Fill in the body text with your own experience while keeping the structural approach of each variant.

## Step 6: Place Your CV PDF

Create the documents/cv/ directory and place your CV inside it:

```
mkdir -p documents/cv
cp /path/to/Your-CV.pdf documents/cv/cv.pdf
```

Make sure the filename matches what you put in `config.private.yaml` -> `documents.cv`.

## Step 7: Place Your Certificates [Optional]

Place any certificate files in `documents/certificates/` and list them in `config.private.yaml` -> `documents.certificates`.

## Step 8: Set LinkedIn URL

Update `config.yaml` -> `platforms.linkedin.search_url` with a fresh LinkedIn job search URL for your target keywords and filters.

## Optional: Toggle Platforms

In `config.yaml` -> `platforms`:
- Set `linkedin.enabled` to `true` or `false`
- Set `web_search.enabled` to `true` or `false`
- Set `linkedin.easy_apply_only` to `true` [only Easy Apply] or `false` [all LinkedIn jobs]

## Optional: Adjust Anti-Detection Timing

In `config.yaml` -> `anti_detection`:
- `delay_between_apps_min_seconds`: minimum delay between applications [default 30]
- `delay_between_apps_max_seconds`: maximum delay between applications [default 300]
- `dwell_time_min_seconds`: minimum time on job description [default 8]
- `dwell_time_max_seconds`: maximum time on job description [default 45]
