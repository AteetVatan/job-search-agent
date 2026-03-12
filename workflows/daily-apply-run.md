---
description: Full LinkedIn Easy Apply session workflow with anti-detection delays
---
// turbo-all

# Daily Apply Run Workflow

## Autonomous Mode

This workflow runs fully autonomously. Do NOT ask for user approval between applications. Only stop and ask the user if:
- CAPTCHA or security challenge appears
- You encounter a question you truly cannot answer
- You get logged out
- An unknown error occurs 3 times in a row

For everything else, proceed without asking.

## Prerequisites

Before starting, verify:
1. `config.yaml` has been updated with today's LinkedIn search URL
2. `config.yaml` -> `platforms.linkedin.enabled` is `true`
3. `config.private.yaml` exists with your personal data
4. You are logged into LinkedIn in the browser

## Workflow Steps

### Step 1: Load Configuration

Read `config.yaml` for:
- `platforms.linkedin.search_url` -> the URL to open
- `job_search.target_roles` -> roles to pursue
- `job_search.exclude_roles` -> roles to skip
- `anti_detection.*` -> all timing parameters

Read `config.private.yaml` for:
- `applicant.*` -> profile data for form fills
- `documents.*` -> CV, certificates, and cover letter paths
- `skills_years` -> years of experience per technology
- `standard_answers` -> exact answers for common form questions

### Step 2: Load Skills

Read and internalize these skills before starting:
- `skills/anti-detection/SKILL.md`
- `skills/linkedin-easy-apply/SKILL.md`
- `skills/humanized-writing/SKILL.md` [AND `skills/humanized-writing/humanized_text_prompt_full.md`]
- `skills/cover-letter-variants/SKILL.md` [AND all files in `skills/cover-letter-variants/variants/`]
- `cv-source-of-truth.md` [for text generation grounding]
- `applicant-profile.md` [for form auto-fill]

### Step 3: Session Start [Anti-Detection Warm-Up]

Per the anti-detection skill, do NOT immediately apply:
1. Open the LinkedIn search URL
2. Spend 30 to 60 seconds browsing the job list
3. Click on 1 to 2 jobs and read their descriptions without applying
4. This establishes a normal browsing pattern

### Step 4: Application Loop

For each job in the list:

1. **Check skip conditions**:
   - Already shows "Applied" -> skip
   - Easy Apply button is disabled or greyed out -> skip
   - No Easy Apply button [only external "Apply"] -> skip [when `easy_apply_only` is true]
   - Job title matches `exclude_roles` -> skip
   - Log every skip with the reason

2. **Dwell on job detail** [anti-detection skill]:
   - Scroll through the job description for the configured dwell time
   - Variable scroll speed, occasional scroll-back
   - Read and understand the job for cover letter variant selection

3. **Execute Easy Apply** [linkedin-easy-apply skill]:
   - Click Easy Apply
   - Fill all form steps using applicant profile data
   - If resume upload required -> upload from `config.private.yaml` -> `documents.cv`
   - If cover letter required -> select variant from cover-letter-variants skill, generate text grounded to CV, apply humanized-writing rules. If file upload needed, save to `documents/cover-letters/` and upload.
   - If certificates/references requested -> upload from `config.private.yaml` -> `documents.certificates`
   - Submit application
   - Close confirmation dialogs

4. **Log result**: Company name, job title, timestamp, "applied" or "skipped [reason]"

5. **Random delay** [anti-detection skill]:
   - Wait random 30 seconds to 5 minutes [from config]
   - During wait, simulate idle browsing behavior

6. **Repeat** for next job in list

### Step 5: Pagination

When all visible jobs on the current page are processed:
1. Scroll to the bottom of the left sidebar
2. Click the next page number [with randomization per anti-detection skill]
3. Continue the Application Loop on the new page

### Step 6: Continue Until Done

- There is NO application cap. Continue until all pages are exhausted or you are stopped.
- If CAPTCHA or throttle is detected -> invoke `workflows/error-recovery.md` immediately

### Step 7: Session Report

After the session ends [all pages done, user stops, or error], report:

1. **Jobs applied to**: company + job title [table]
2. **Jobs skipped**: company + job title + skip reason [table]
3. **Current page number**
4. **Total applications submitted**
5. **Total jobs skipped**
6. **Any errors or issues** that need user input
