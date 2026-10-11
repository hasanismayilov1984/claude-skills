---
name: keywords
description: Tailor a resume to a job description for ATS keyword matching. Asks the user to paste the job description, extracts its keywords, scores the resume against them, and rewrites the resume to include missing keywords truthfully. Use when the user types keywords, key-word, tailor resume, or match resume to a job.
---

# Keywords: tailor resume to a job description

## Step 1 — Ask for the job description (always first)

Do not do anything else first. Ask exactly:

> Paste the job description (the whole posting is fine). Optionally add the company name and job title on the first line.

Wait for the user's reply. If the reply is a link instead of text, ask them to paste the text — many job sites block automated reading.

## Step 2 — Find the resume

Look in the current repo, in this order:
1. `resumes/master-resume.md`
2. Any other file in `resumes/` (`.md`, `.txt`, `.docx`, `.pdf`)
3. Any file with "resume" or "cv" in its name

If there are several, list them and ask which one to use. If none, ask the user for a path or to paste the resume.

Reading `.docx`: `pandoc "<file>" -t markdown`. Reading `.pdf`: `pdftotext -layout "<file>" -`.

## Step 3 — Extract keywords from the job description

Pull out and group:
- **Job title** and close variants
- **Hard skills / tools** (e.g. Power BI, DAX, Microsoft Fabric, SQL, Python, Azure, dbt, RAG, LLM)
- **Methods / concepts** (e.g. dimensional modeling, data governance, ETL, A/B testing)
- **Domain** (e.g. financial services, healthcare, supply chain)
- **Soft skills / responsibilities** (e.g. stakeholder management, mentoring)
- **Requirements** (years of experience, degree, certifications, clearance)

Mark each as **Required** (in "requirements"/"must have", or repeated) or **Preferred**. Keep the job description's exact wording — ATS systems match literal phrases.

## Step 4 — Score the current resume

Show a table:

| Keyword | Required/Preferred | In resume? | Where |
|---|---|---|---|

"In resume?" is ✅ exact match, 🟡 similar wording (e.g. "PBI" vs "Power BI"), or ❌ missing.

Then a match score: required keywords matched / total required, and the same for preferred.

## Step 5 — Check missing keywords with the user

For every ❌ keyword, decide:
- **Clearly supported** by the resume under different words → will reword to the exact phrase.
- **Not evident** from the resume → ASK the user: "Do you have experience with X? If yes, where (which job, what you did)?"

**Never add a skill, tool, certification, degree, or number the user hasn't confirmed.** Fabricated keywords fail in interviews and background checks. If the user says no, leave it out and list it under "gaps" at the end.

Ask about all unclear keywords in one message, not one at a time.

## Step 6 — Rewrite the resume

Edit a copy, never the original. Apply in this priority:
1. **Title / headline** — mirror the job title if it's truthful.
2. **Summary** — work in the top 4–6 required keywords naturally.
3. **Skills section** — add confirmed keywords; order to match the job description's priorities; write acronyms both ways once, e.g. "Extract, Transform, Load (ETL)".
4. **Experience bullets** — swap in exact phrasing where the meaning is the same; add confirmed keywords to the most relevant job; keep metrics unchanged.

Rules:
- Keep the original structure, section order, and length (one extra line per section at most).
- No keyword stuffing: each keyword 1–3 times total, always in a real sentence or the skills list.
- Keep the user's voice; don't add buzzwords the job description doesn't use.

## Step 7 — Save and report

Save to `tailored/YYYY-MM-DD-company-role.md` (create the folder if needed; use lowercase-hyphenated names). If the original was `.docx`, also create a `.docx`:
`pandoc tailored/<name>.md -o tailored/<name>.docx` — and if `resumes/reference.docx` exists, add `--reference-doc resumes/reference.docx` to keep the original styling.

Then report, briefly:
- Match score **before → after** (required and preferred)
- What changed (bullet list, one line each)
- **Gaps**: keywords left out because the user doesn't have them, with a one-line tip for addressing each in a cover letter or interview
- Ask whether to commit the tailored resume to git
