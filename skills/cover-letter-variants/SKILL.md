---
name: cover-letter-variants
description: CV-grounded cover letter variants with selection logic
---

# Cover Letter Variants Skill

## Purpose

Generate cover letters that are structurally different from each other while using ONLY facts from `cv-source-of-truth.md`. Each variant has a different tone, structure, and emphasis.

## HARD RULE: NO HALLUCINATION

Every single claim, project name, technology, number, and achievement in any generated cover letter MUST exist in `cv-source-of-truth.md`. If you cannot find a fact there, do not write it.

Specifically:
- Do NOT invent company names, client names, or team sizes not in the CV
- Do NOT add technologies not listed in Technical Skills
- Do NOT claim certifications not listed
- Do NOT embellish metrics [e.g., do not change "40%" to "50%"]
- If the job description mentions a skill not in the CV, do NOT pretend to have it. Either skip mentioning it or write "I do not have direct experience with [X] but my background in [related Y from CV] positions me well to pick it up fast."

## Variant Selection Logic

Read the job description and company context, then select:

| Signal | Use Variant |
|---|---|
| Company under 50 people, startup vibe, Wellfound | `variant_startup.md` |
| Title includes "Lead", "Head", "Manager", "Director" | `variant_leadership.md` |
| JD is heavily technical, lists specific frameworks and tools | `variant_technical.md` |
| JD mentions a project domain matching one of the CV projects | `variant_project.md` |
| Default, unclear, or enterprise company | `variant_direct.md` |

If multiple signals match, prefer in order: startup > leadership > technical > project > direct.

## Customization Per Job

After selecting a variant:
1. Replace [JOB TITLE] with the actual job title
2. Replace [COMPANY NAME] with the actual company name
3. If the JD mentions a specific technology that appears in the CV, add one sentence referencing your experience with it
4. Apply the humanized-writing skill to the final text
5. Keep total length under 200 words [or as set in `config.yaml` → `text_generation.max_cover_letter_words`]

## Variant Files

- `variants/variant_direct.md` — short, punchy, gets to the point
- `variants/variant_technical.md` — deep dive on one technical area
- `variants/variant_startup.md` — founder mentality, building from zero
- `variants/variant_leadership.md` — team building, architecture strategy
- `variants/variant_project.md` — highlights a specific matching project
