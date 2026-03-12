---
description: Handle CAPTCHAs, throttling, and session errors during job application sessions
---

# Error Recovery Workflow

## Purpose

This workflow is invoked automatically when the daily-apply-run or job-search-run workflows encounter an error. It defines exact actions for each error type. The primary rule is: when in doubt, STOP and notify the user.

## Error Handlers

### CAPTCHA Detected

Any CAPTCHA challenge [image grid, text entry, slider puzzle, "I am not a robot" checkbox]:
1. Take a screenshot of the CAPTCHA
2. **STOP the entire session immediately**
3. Report to user: "CAPTCHA detected. Session stopped. Screenshot attached."
4. Do NOT attempt to solve the CAPTCHA
5. Do NOT dismiss or close the CAPTCHA dialog

### "Unusual Activity" Warning

A banner, modal, or message containing "unusual activity", "verify your identity", "security check", or similar:
1. Take a screenshot
2. **STOP the entire session immediately**
3. Report to user: "Security warning detected. Session stopped. Screenshot attached."
4. Do NOT dismiss the warning
5. Do NOT click any buttons on the warning

### Easy Apply Dialog Stuck

The Easy Apply form is open but a step will not advance [button click does nothing, loading spinner for 10+ seconds]:
1. Click the X button to close the dialog
2. Wait 3 seconds
3. Verify the job list is still visible
4. If yes → log "skipped [form stuck]" and continue to the next job
5. If no [page broke] → refresh the page → wait for job list to reload → continue

### Logged Out

LinkedIn redirects to the login page or shows "session expired":
1. **STOP the entire session immediately**
2. Report to user: "Logged out of LinkedIn. Session stopped."
3. Do NOT attempt to log back in

### Form Validation Error

A required field is highlighted red, or a "Please fill in this field" message appears, and the correct answer is not clear:
1. Take a screenshot of the error
2. Close the Easy Apply dialog [X]
3. Log "skipped [form validation error]" with the screenshot
4. Continue to the next job

### Page Not Loading

The LinkedIn page or an external page shows a loading spinner, blank page, or error for 15+ seconds:
1. Wait 10 seconds
2. Refresh the page
3. Wait 10 seconds
4. If still broken → **STOP the session** and report: "Page not loading. Session stopped."
5. If recovered → continue from where you left off

### Rate Limit / Too Many Requests

A "429", "Too Many Requests", or "Rate Limited" message appears:
1. Take a screenshot
2. **STOP the entire session immediately**
3. Report to user: "Rate limited. Session stopped. Try again in a few hours."

### Unknown Error

Any error not covered above:
1. Take a screenshot
2. Log the error description
3. Close any open dialogs
4. Try to continue to the next job
5. If the same unknown error occurs 3 times in a row → **STOP the session** and report all 3 occurrences
