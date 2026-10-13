# Context Bridge

**Context Bridge helps AI use the work and personal context you approve, across the tools where your life is split.**

Your AI starts from zero in each tool, while the context it needs is scattered across work and personal systems. Context Bridge is a permission-first way to carry the right context to a task while keeping the boundary visible.

- [Open the live Context Bridge prototype](https://smadden2-gif.github.io/genai-practice/)
- [View the public GitHub repository](https://github.com/smadden2-gif/genai-practice)

## The first slice

The page centers on one simulated flow: enter a work question, add optional personal context, explicitly approve whether that context may be considered, and optionally write a separate note to keep private. The result shows what personal context was used and what stayed out. Approval is unchecked by default.

## AI role and transparency

The prototype does not call an AI model. Its response is a fixed browser-side template that includes the work question and, only if the approval box is checked, the personal context. The separate private-note field is never rendered into the result. There is no backend, API key, storage, or network transfer of entered text. This tests the permission and audit interaction, not the quality of an AI-generated answer or a real cross-tool integration.

## Quick tests

These cases were run against the live Pages prototype after the Context Bridge redesign:

| Test | Input or action | Expected behavior | Result |
| --- | --- | --- | --- |
| Typical | Enter a work question, personal context, check approval, and add a separate private note. | Response includes the work question and approved context; Used lists the approved context; Kept out says the private note stayed out without revealing its text. | Pending live retest |
| Challenge | Enter a difficult work question and personal context; leave approval unchecked; add a private note. | Response uses the no-context template; Used says no personal context used; the private note stays out. | Pending live retest |
| Invalid or empty | Reload to reset the result, leave required work input empty, and submit. | Browser required-field validation blocks submission; no result is revealed. | Pending live retest |

## Known limitations

- The response is a fixed template. It does not understand the work question or reason over context. Next: test with people, then evaluate a model using synthetic examples and a clear boundary for approved input.
- The demo does not save decisions or remember permission between visits. Next: define user controls and deletion before considering storage.
- It does not connect to other tools or move context between them. Next: test the permission model before adding an integration, and show exactly what data an integration would send.

## Run locally

Clone the repository and open `index.html` in a browser. No build step or server is required. Lexend loads from Google Fonts when online.

```bash
git clone https://github.com/smadden2-gif/genai-practice.git
cd genai-practice
open index.html
```
