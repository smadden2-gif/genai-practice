# GenAI Practice

A small AI product experiment repo. Its first project is **Continuity**, an idea for helping people act on work opportunities while keeping the commitments and personal context they choose to protect.

## Continuity

**Lower the cost of saying yes.**

This page explores a product that could help people take opportunities without letting the rest of life unravel. The first working slice is a small context bridge: enter a work decision, choose whether personal context may be considered, and see a suggested next step with a clear record of what was used and what stayed out.

- [Open the GitHub Pages prototype](https://smadden2-gif.github.io/genai-practice/)
- [View the public GitHub repository](https://github.com/smadden2-gif/genai-practice)

## Try it

Open the prototype, scroll to **Try the first slice**, enter a work decision, and optionally add personal context. Check the approval box only if you want that text used in the result. Text in the private field is never used. The result shows which personal context crossed the bridge and which did not.

## AI role and what works

The future AI would help connect a work opportunity with relevant personal context, but only context the person has approved for that decision. It could suggest a next step while keeping unrelated or unapproved context out.

The browser flow works: it accepts text, checks explicit permission for one field, and displays a result and a used/not-used record. The recommendation is a fixed, hard-coded template, not an AI response. There is no model call, backend, API key, data storage, or network transfer of entered text. The original landing page's email form still does not save or send addresses, and its chart is illustrative rather than measured data.

## Quick tests

I ran these three cases on the live GitHub Pages site:

| Test | Input or action | What happened | Result |
| --- | --- | --- | --- |
| Typical | Work: "I could take a three-month role in Singapore." Approved context: "I can travel after my sibling finishes school in June." Check the approval box; private note: "Family medical details." | It showed a fixed next-step response using the approved travel timing. The Used panel showed that timing; Kept out said a separate private note was left out. The note itself did not appear in the result. | Pass |
| Challenge | Work: "Maybe I should quit tomorrow?" Personal context: "I may need to care for my parent." Leave approval unchecked; private note: "Do not use this." | It showed the reversible-step template, said no personal context was approved, showed "No personal context used," and kept the private note out. | Pass |
| Invalid or empty | Reload the live page so the result starts hidden, leave the required work-decision box empty, and submit. | The browser's required-field check blocked submission; the result stayed hidden. | Pass |

## Known limitations

- The bridge response is a fixed template, so it does not understand the work decision or reason over context. Next: test the flow with people, then consider a model that receives only explicitly approved context.
- The demo does not save decisions or remember boundaries between visits. Next: only add storage after deciding how to give users clear control and deletion.
- This is a single-page prototype; it does not coordinate calendars, commitments, or actions in other services. Next: learn which one action would be most useful before adding integrations.

## Run locally

Download or clone this repository and open `index.html` in a browser. No build step or server is required. Lexend loads from Google Fonts when online.

```bash
git clone https://github.com/smadden2-gif/genai-practice.git
cd genai-practice
open index.html
```

The GitHub Pages link is for this prototype copy. The earlier live concept site at `https://continuity.simonmaddenbcc.chatgpt.site` is separate and is not updated by changes in this repository.
