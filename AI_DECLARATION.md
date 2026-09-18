# AI-use declaration

## Tools used

- **Claude (Anthropic)** — used to design and write the full application
  (`index.html`), including the UI, the state management, the Gemini API
  integration, the prompt templates for quiz generation / gap
  classification / lesson generation, and this documentation.

## What was personally verified

- Read through the full JS logic end-to-end and confirmed the diagnostic →
  gap-map → lesson → practice → mastery-update flow matches the intended
  product design.
- Checked the JS for syntax errors (`node --check`) before submission.
- Manually walked through **Demo Mode** to confirm the gap map, lesson, and
  practice screens render correctly with no API key and no network calls.
- Reviewed the Gemini prompt templates for correctness of the requested
  JSON shape, and confirmed the app has a fallback path (regex extraction)
  if the model wraps JSON in extra text.
- Confirmed no API keys or secrets are hard-coded anywhere in the source —
  each user supplies their own Gemini key at runtime, stored only in
  their browser's `localStorage`.
- Tested error states: missing API key, network failure, and malformed
  AI response each show a distinct message with a Retry button rather
  than a blank screen or silent failure.

## What still needs live testing before submission

- A real diagnostic run end-to-end with a live Gemini key on 2–3 different
  topics, to sanity-check the quality and difficulty of AI-generated
  questions.
- Mobile-width testing on an actual phone, not just a resized browser
  window.
