---
description: Discover new jobs across web platforms using LLM-powered search
---
// turbo-all

# Job Search Run Workflow

## Prerequisites

Before starting, verify:
1. `config.yaml` → `platforms.web_search.enabled` is `true`
2. `config.yaml` → `platforms.web_search.search_engines` has at least one platform listed
3. You have browser access to the listed platforms

## Workflow Steps

### Step 1: Load Configuration

Read `config.yaml` for:
- `job_search.target_roles` → what to search for
- `job_search.exclude_roles` → what to filter out
- `job_search.locations` → where to search
- `job_search.keywords` → search terms
- `platforms.web_search.search_engines` → which platforms to use
- `platforms.web_search.freshness` → how recent jobs should be

### Step 2: Load Skills

Read and internalize:
- `skills/job-web-search/SKILL.md`
- `skills/anti-detection/SKILL.md` [for delays between searches]

### Step 3: Build Search Queries

Using target_roles, locations, and keywords from config, construct 5 to 10 search queries. Mix roles with locations:
- "[AI Engineer] [Germany] jobs"
- "[GenAI Architect] [remote EU]"
- "[Head of AI] [Netherlands]"
- "[LLM Engineer] [remote] [Europe]"

### Step 4: Execute Searches

For each platform in search_engines:
1. Navigate to the platform
2. Execute each search query
3. Apply freshness filter [e.g., last 7 days]
4. Extract job listings: title, company, location, posted date, apply URL
5. Respect anti-detection delays between search queries

### Step 5: Filter and Score

Per the job-web-search skill:
1. Remove jobs matching `exclude_roles`
2. Remove jobs outside configured `locations`
3. Score each remaining job 1 to 10 based on relevance
4. Deduplicate cross-platform results

### Step 6: Present Results

Output a ranked table of discovered jobs:

| Score | Title | Company | Location | Platform | Apply URL |

Group into:
1. **LinkedIn Easy Apply available** → recommend running daily-apply-run workflow
2. **External direct apply** → show URL, awaiting user decision
3. **Manual steps required** → flag for user attention

### Step 7: User Decision

Present the results and wait for user input:
- Which jobs to apply to
- Whether to queue LinkedIn jobs for the daily-apply-run workflow
- Whether to proceed with any external applications
