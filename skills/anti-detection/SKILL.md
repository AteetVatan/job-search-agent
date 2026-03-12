---
name: anti-detection
description: Human-like browser behavior patterns to avoid bot detection on job platforms
---

# Anti-Detection Skill

## Purpose

This skill defines browser behavior rules that make automated job applications look like normal human activity. Load this skill before every browser interaction during a job application session.

## Inter-Application Delay

**MANDATORY**: After completing each job application [submit or skip], wait a random duration before starting the next one.

- Minimum wait: read from `config.yaml` → `anti_detection.delay_between_apps_min_seconds` [default 30 seconds]
- Maximum wait: read from `config.yaml` → `anti_detection.delay_between_apps_max_seconds` [default 300 seconds / 5 minutes]
- The delay must be truly random within this range, not clustered around the midpoint

During the wait period, simulate idle behavior on the current page:
- Scroll up and down slowly on the job list
- Hover over random job titles without clicking
- Occasionally scroll to a different part of the page

## Job Description Dwell Time

Before clicking Easy Apply or deciding to skip a job, spend time on the job detail page:

- Minimum dwell: read from `config.yaml` → `anti_detection.dwell_time_min_seconds` [default 8 seconds]
- Maximum dwell: read from `config.yaml` → `anti_detection.dwell_time_max_seconds` [default 45 seconds]
- During this time:
  - Scroll through the entire job description at variable speed [not constant]
  - Pause on sections that contain keywords from `config.yaml` → `job_search.keywords`
  - Occasionally scroll back up to re read a section
  - Do NOT just wait motionless. Move the scroll position.

## Scroll Behavior

Never scroll at a constant speed. Use this pattern:
- Fast scroll through boilerplate [company overview, benefits sections]
- Slow scroll through requirements and responsibilities
- Occasional pause [2 to 5 seconds] mid scroll as if reading a specific line
- Sometimes scroll past something and then scroll back up slightly

## Pagination Randomization

When moving between pages of job results:
- Do NOT always click the next sequential page number
- About 70% of the time, go to the next page normally
- About 15% of the time, go back to re check a previous page
- About 15% of the time, skip one page forward then come back
- Never traverse more than 10 pages in perfectly sequential order without at least one deviation

## Click Variance

- Do NOT click dead center on buttons. Click at a slightly randomized position within the button area.
- Vary the speed between mouse move and click [sometimes fast, sometimes a brief hover before clicking]

## CAPTCHA and Throttle Detection

If any of the following appear, STOP THE ENTIRE SESSION IMMEDIATELY and notify the user:

1. Any CAPTCHA challenge [image selection, text entry, puzzle]
2. A banner or message containing "unusual activity" or "verify you are human" or "security check"
3. The Easy Apply button disappears from all jobs on a page [possible throttle]
4. You get logged out unexpectedly
5. A "rate limit" or "too many requests" message appears

Do NOT attempt to solve CAPTCHAs. Do NOT dismiss security warnings. Screenshot the issue and stop.

## Session Start Behavior

At the start of each session, do NOT immediately begin applying:
- Spend 30 to 60 seconds browsing the job list casually
- Click on 1 to 2 jobs and read their descriptions without applying
- This establishes a normal browsing pattern before the apply sequence begins
