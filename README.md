# Founding Teachers 2027 — Application form

Application form (Italian only) for the **Founding Teacher** position at Italienska Skolan
Stockholm. Same visual style as the Scuola del Sabato form. Static HTML frontend (GitHub Pages)
+ Google Apps Script backend, the same architecture as `scuola-del-sabato-form`.

| File | Purpose |
|------|---------|
| `founding-teachers.html` | The form (HTML + CSS + JS in one file). |
| `privacy.html` | Privacy notice, linked from checkbox F2. |
| `Code.gs` | Apps Script backend: ID, CV on Drive, KO/FLAG, sheet row, duplicates, emails. **Not** for GitHub Pages. |
| `logo-italienska-skolan.png` | School logo. |

## Setup

1. **Apps Script, from the careers@italienskaskolan.se account.** That account then owns the
   Sheet and the CV folder, so only whoever manages careers@ can see the data.
   - New project → paste `Code.gs` → Project settings → time zone *Europe/Stockholm*.
   - Run `setup()` once: it creates the **Founding Teachers 2027 — Candidature** Sheet and the
     **Founding Teachers 2027 — CV** Drive folder (URLs in the log) and asks for authorisation.
   - Deploy → New deployment → Web app (Execute as: **Me**, Access: **Anyone**). Copy the `/exec` URL.
2. **Frontend:** paste the `/exec` URL into `FT_CONFIG.appsScriptUrl` in `founding-teachers.html`,
   then publish `founding-teachers.html`, `privacy.html` and the logo on GitHub Pages.
3. **Test:** set `TEST_EMAIL` in `Code.gs` and run `testSubmission()`, or submit the real form.
4. **Before going live:** delete the test rows (keep row 1) and run `resetCounter()` so the first
   real application is `FT27-001`.

After any change to `Code.gs`: Deploy → Manage deployments → ✏️ → **New version**.

## How it works

- **Links per channel:** `founding-teachers.html?src=linkedin` (also `indeed`, `website`, `eures`,
  any value). No parameter → `diretto`.
- **Automatic closing:** Sunday 1 November 2026, 23:59 (Europe/Stockholm), enforced both in the page
  and in `doPost`. After that the page shows the "modulo chiuso" message.
- **ID:** `FT27-001`, `FT27-002`, … from a persistent counter (deleting rows never reuses an ID).
- **KO / FLAG:** computed in `computeLabels_()` and written as e.g. `KO: B4, B7` / `FLAG: B1`.
  The applicant sees nothing.
- **Duplicates:** same email twice → both rows kept, the older one gets
  `duplicato (più recente: FT27-0xx)` in the *Duplicato* column.
- **CV:** PDF only, max 5 MB (checked in the browser and on the server), saved to the private
  Drive folder as `FT27-001 – Cognome Nome – CV.pdf`; the *C4 CV* column links to it. Never attached
  to an email.
- **Emails:** careers@ gets a notification (name, ID, KO/FLAG, link to the row); the applicant
  gets the confirmation from careers@, with a copy of their answers.
- **Sheet columns:** in the order of the spec; B2 has one column group per title (3 × 6).
  C2 (teaching positions) and the B2 final grade were removed from the form; the other question
  codes are unchanged (C3, C4, …).

## Second form: Leg. Grundskollärare F–3 (Swedish)

Same design and architecture, in Swedish, with its **own** Apps Script project, Sheet, CV folder
and ID counter (`GL27-001`, …). Deadline: Sunday 6 December 2026, 23:59 (CET).

| File | Purpose |
|------|---------|
| `grundskollarare-f3.html` | The form, with the job ad on top. |
| `integritetspolicy.html` | Privacy notice in Swedish, linked from F2. |
| `Code-grundskollarare.gs` | Its Apps Script backend (git-ignored, like `Code.gs`). |

Setup is the same as for the Italian form: new Google Sheet from the careers@ account →
Extensions → Apps Script → paste `Code-grundskollarare.gs` → run `setup()` → Deploy as Web app →
paste the `/exec` URL into `FT_CONFIG.appsScriptUrl` in `grundskollarare-f3.html`.
The KO/FLAG rules in `computeLabels_()` are a proposal (the draft had none) — see the comment there.

`wix-posizioni-aperte.html` (local only, pasted into the Wix "Lavora con noi" page) lists both
positions; each card moves from "Posizioni aperte" to "Candidature chiuse" at its own deadline.
