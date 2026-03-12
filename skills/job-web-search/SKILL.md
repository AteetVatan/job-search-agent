---
name: job-web-search
description: LLM-powered job discovery across web platforms using browser and search
---

# Job Web Search Skill

## Purpose

Discover relevant job postings across the web beyond LinkedIn. Uses browser subagent to search job platforms and LLM reasoning to extract, filter, and rank results. This skill finds jobs. It does not auto-apply.

## Search Strategy

### Step 1: Build Search Queries

Read from `config.yaml`:
- `job_search.target_roles` → primary search terms
- `job_search.locations` → location filters
- `job_search.keywords` → additional keywords

Construct search queries by combining roles with locations. Examples:
- "AI Engineer Germany remote"
- "GenAI Architect EU"
- "Head of AI Netherlands"
- "LLM Engineer remote Europe"

Generate 5 to 10 diverse query combinations, mixing roles and locations.

### Step 2: Search Platforms

For each platform in `config.yaml` → `platforms.web_search.search_engines`:

**Google Jobs**:
- Navigate to Google, search "[query] jobs"
- Google will show a job listings panel
- Apply freshness filter from config [e.g., last 7 days]
- Extract visible job cards [title, company, location, posted date]

**Indeed**:
- Navigate to indeed.com or indeed.de
- Enter query in "What" field, location in "Where" field
- Filter by date posted [last 7 days]
- Extract job listings from results

**Glassdoor**:
- Navigate to glassdoor.com
- Search by role title and location
- Extract job listings

**Wellfound [AngelList Talent]**:
- Navigate to wellfound.com/jobs
- Filter by role, location, and remote
- Extract startup job listings

### Step 3: Extract Structured Data

For each job found, extract:
- Job title
- Company name
- Location [city, country, or "remote"]
- Posted date or freshness [e.g., "2 days ago"]
- Apply URL [direct link to application page]
- Source platform

### Step 4: Filter

Remove jobs that match `config.yaml` → `job_search.exclude_roles`. Check title against exclusion list:
- If title contains "ML Engineer" or "Data Scientist" or any excluded term, discard
- If title matches "target_roles", mark as high relevance

Remove jobs outside configured locations:
- If location does not match any entry in `config.yaml` → `job_search.locations`, discard
- "Remote" jobs are kept if "Remote [EU]" or "Remote [Global]" is in locations

Remove jobs that require fluency in languages the applicant does not speak:
- Check `config.yaml` → `job_search.language_filter`
- If the job listing explicitly requires fluency in a language from `skip_if_required` [e.g., "fluent French required", "must speak Dutch"], discard
- If German is required at B2 or higher [C1, fluent, native], discard [applicant has B1 German only]
- If German B1 or lower is required, keep
- If a non-accepted language is listed as "nice to have" or "preferred", keep
- If no language requirement is mentioned, keep [assume English is fine]

### Step 5: Score and Rank

Score each remaining job 1 to 10:
- +3 if title exactly matches a target role
- +2 if title partially matches [e.g., "Senior AI Engineer" matches "AI Engineer"]
- +2 if location is in Germany
- +1 if location is EU
- +1 if posted within last 3 days
- +1 if company is a startup [small team signals from Wellfound]

Sort by score descending.

### Step 6: Present Results

Output a table:

| Score | Title | Company | Location | Platform | URL |
|---|---|---|---|---|---|

Group by:
1. Jobs with LinkedIn Easy Apply available [recommend using daily-apply-run workflow]
2. Jobs with direct external applications [show apply URL for manual or future auto-apply]
3. Jobs requiring manual steps [e.g., email-only applications]

## Deduplication

If the same job appears on multiple platforms [same company + same title + same location], keep only the version with the most direct apply path. Prefer LinkedIn Easy Apply over external forms.

## Important Constraints

- This skill DISCOVERS jobs. It does NOT auto-apply to external sites.
- Do not click "Apply" on external sites unless explicitly told to by the user.
- Present findings and let the user decide which to pursue.
- Respect `config.yaml` → `anti_detection` delays between search queries.
