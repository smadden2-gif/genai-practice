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

These cases were run against the redesigned live Pages prototype at https://smadden2-gif.github.io/genai-practice/:

| Test | Input or action | Expected behavior | Result |
| --- | --- | --- | --- |
| Typical | Work: "I could lead a launch while keeping weekly family commitments." Approved context: "I have a standing appointment on Thursday evenings." Check approval; private note: "Family health details I will not share here." | Result included the work question and approved context; Used showed the Thursday commitment. Kept out said the private note was excluded without revealing its text. | Pass |
| Challenge | Work: "Should I accept a role with frequent travel?" Personal context: "I may need to care for my parent." Leave approval unchecked; private note: "Do not use this." | No personal context was approved; the template used the work question, Used said no personal context was used, and Kept out described the private note without exposing it. | Pass |
| Invalid or empty | Reload the page to start with a hidden result, leave required work input empty, and submit. | Browser required-field validation displayed "Please fill out this field." Result stayed hidden. | Pass |

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
