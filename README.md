# GapTutor — Find what you're missing

**Track:** AI for Learning — CodeMyFYP Hackathon
**Live demo:** https://gaptutor.vercel.app

**Video walkthrough:** _add your pitch video link here_

## Problem

Most study tools quiz students on everything, wasting time re-covering things
they already know. Students don't need more content — they need to know
**exactly which sub-concept** they're weak in, and a fast way to fix it.

## Who it's for

Students revising a chapter or topic before an exam who want a focused,
5-minute diagnosis of their weak spots instead of a full re-read.

## What it does

1. **Diagnose** — the student names a topic. GapTutor (via Google's Gemini
   API) generates a 6-question diagnostic quiz covering 3 distinct
   sub-concepts.
2. **Visualize** — answers are scored per sub-concept into a **Gap Map**:
   a mastery percentage (0–100%) for each sub-concept, plus an AI-generated
   classification of *why* the student got it wrong (conceptual
   misunderstanding / missing prerequisite / careless error).
3. **Fix** — for any weak sub-concept, GapTutor generates a short
   micro-lesson (analogy, explanation, worked example, common trap) and
   3 fresh practice questions of increasing difficulty. Practice updates
   the mastery score live; hitting 80%+ marks the gap closed, with a small
   confetti celebration.
4. **Track** — a dashboard remembers every topic diagnosed, per-browser,
   so a student can see mastery trend over multiple study sessions.

Includes a **Demo Mode** on the home screen that loads a pre-scored
DBMS-Normalization example instantly, with no API key required — for quick
judging.

## Architecture

Single self-contained web app (`index.html`) — plain HTML/CSS/vanilla JS,
no build step or framework required.

- **UI**: hand-written CSS (no framework), Google Fonts (Fraunces + Inter +
  JetBrains Mono).
- **AI**: Google Gemini API (`gemini-2.0-flash`), called directly from the
  browser with `fetch`. Structured JSON output is requested via
  `responseMimeType: "application/json"` for the quiz, gap-classification,
  and lesson-generation calls.
- **State**: in-memory app state for the current session; `localStorage`
  for the student's Gemini API key and their per-topic mastery history
  (nothing is sent to any server other than Google's Gemini endpoint).

```
index.html        the entire application
README.md         this file
AI_DECLARATION.md  AI tools used + what was verified
```

## Setup / running locally

No install, no build step.

1. Get a **free** Gemini API key at
   [aistudio.google.com/apikey](https://aistudio.google.com/apikey)
   (no credit card required).
2. Open `index.html` in any modern browser (or open the deployed link).
3. Click the ⚙ settings icon top-right, paste in the API key, save.
4. Type a topic (e.g. "Newton's Laws of Motion") and click **Run
   Diagnostic** — or click **Try Demo Mode** to see the full flow with no
   key at all.

### Deploying

This is a static site — drag-and-drop `index.html` onto
[Vercel](https://vercel.com) or [Netlify](https://netlify.app), or enable
GitHub Pages on this repo. No environment variables or server config
needed, since each student supplies their own Gemini key client-side.

## Limitations

- The Gemini API key is entered by each user and stored only in their own
  browser (`localStorage`) — there's no backend, so there's nothing to
  leak server-side, but the key **is visible in that browser's dev tools**,
  which is acceptable for a personal study tool but not for a
  multi-tenant production product.
- No accounts / no cross-device sync — progress is per-browser.
- Gemini's free tier has a requests-per-minute limit; heavy classroom-scale
  use would need a paid tier or a backend proxy with rate limiting.
- Gap classification (conceptual vs. careless vs. missing-prerequisite) is
  AI-inferred from a single wrong answer and is a best-effort signal, not
  a certified diagnosis.

## Roadmap

- Accounts + server-side progress sync across devices
- Upload lecture notes / a syllabus PDF to scope the diagnostic more precisely
- Spaced-repetition scheduling for previously-closed gaps
- Teacher view: aggregate gap maps across a whole class
