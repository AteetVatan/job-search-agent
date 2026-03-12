---
name: linkedin-easy-apply
description: Step-by-step LinkedIn Easy Apply form handling procedures
---

# LinkedIn Easy Apply Skill

## Purpose

This skill handles the mechanics of filling and submitting LinkedIn Easy Apply forms. It is invoked by the orchestrator for each job that has Easy Apply available.

## Pre-Apply Checks

Before clicking Easy Apply on any job, verify ALL of the following:

1. The job does NOT show "Applied" badge
2. The Easy Apply button is visible and clickable [not greyed out, not disabled]
3. The button says "Easy Apply" [not just "Apply" which redirects externally]
4. The job title does NOT match any role in `config.yaml` -> `job_search.exclude_roles`
5. If the title matches `config.yaml` -> `job_search.target_roles`, that is a positive signal

6. The job description does NOT require fluency in a language the applicant does not speak. Check `config.yaml` -> `job_search.language_filter`:
   - Read the job description text before applying
   - If the JD says a language from `skip_if_required` is **required** [e.g., "fluent French required", "must speak Dutch", "native Spanish speaker needed"] -> skip the job
   - If the JD says German is required at B1 or lower -> that is fine [applicant has B1 German]
   - If the JD says German is required at B2, C1, or "fluent" -> skip [applicant only has B1]
   - If a non-accepted language is listed as "nice to have", "preferred", "a plus" -> do NOT skip
   - If no language requirement is mentioned -> do NOT skip [assume English-only is fine]

If any check fails, skip the job. Log the skip reason.

## Prioritization

If a job shows "Job match is high" or similar LinkedIn match indicators, apply to it before lower match jobs on the same page.

## Form Handling

### Multi-Step Forms

LinkedIn Easy Apply forms can have 1 to 6 steps. Handle each step:

1. **Contact info step**: Usually pre-filled from LinkedIn profile. Verify fields are correct. If email or phone is empty, fill from `applicant-profile.md`. Click Next.

2. **Resume / CV upload step**:
   - If it asks to **select** a previously uploaded resume: select the most recent one. Click Next.
   - If it asks to **upload** a resume file: upload from `config.private.yaml` -> `documents.cv`. Click the upload button or drag-and-drop zone to trigger the file chooser. Click Next.
   - If a resume is already attached from a previous application: keep it. Click Next.

3. **Additional questions step**: This is the most variable step. Handle each field type:
   - **Dropdown [select]**: Choose the most appropriate option. Use `applicant-profile.md` -> Standard Form Answers for common questions.
   - **Radio buttons**: Select the best matching answer using Standard Form Answers.
   - **Text input [short]**: Fill using values from `applicant-profile.md` -> Skills table or Standard Form Answers.
   - **Text input [years of experience]**: Look up the exact skill in `config.private.yaml` -> `skills_years`. If the skill is not listed but plausible, use 3. If unknown, use 0.
   - **Text area [long form]**: Use `cv-source-of-truth.md` as the ONLY source. Apply the humanized-writing skill. Keep to 2 to 4 sentences.
   - **File upload [certificates, references, transcripts]**: Check `config.private.yaml` -> `documents.certificates` for matching files. If a suitable file exists in `documents/certificates/`, upload it. If no matching file exists, skip the field if optional, or log "certificate upload required" and skip the job.

4. **"Top Choice" optional page**: Just click Next without selecting anything.

5. **Cover letter step**:
   - If the cover letter field is **optional**: skip it, click Next
   - If the cover letter field is a **required text box**: load `skills/cover-letter-variants/SKILL.md`, select the appropriate variant, generate the text, paste it. Apply humanized-writing skill.
   - If the cover letter field requires a **file upload**:
     1. Load `skills/cover-letter-variants/SKILL.md` and generate the cover letter text
     2. Save the text as a file to `documents/cover-letters/cover_letter_[company_name].txt`
     3. Upload the file via the form's file upload field
     4. The file will persist in `documents/cover-letters/` for the user's records

6. **Review and Submit step**: Click Submit Application.

### Message to Recruiter / Additional Information

If a free text field labeled "Message to recruiter" or "Additional information" appears:
- Write 2 to 3 sentences about why you are a fit, using ONLY facts from `cv-source-of-truth.md`
- Include portfolio link only if contextually relevant [not every single time]
- Apply humanized-writing skill

### Error Recovery

- If a required field is highlighted red and you cannot determine the correct answer: close the dialog [X button] and skip the job
- If the form is stuck loading for more than 10 seconds: close and skip
- If a "Something went wrong" error appears: close and skip
- After closing a stuck dialog, verify the job list is still visible. If not, refresh the page.

### Post-Submit

After successful submission:
1. Close any "Application submitted" confirmation dialog
2. Close any "Follow company?" dialog
3. Log: company name, job title, timestamp, "applied"
4. Return to the job list
