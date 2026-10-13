# Context Bridge

**Context Bridge helps AI use the work and personal context you approve, across the tools where your life is split.**

Your AI starts from zero in each tool, while the context it needs is scattered across work and personal systems. Context Bridge is a permission-first way to carry the right context to a task while keeping the boundary visible.

- [Open the live Context Bridge prototype](https://smadden2-gif.github.io/genai-practice/)
- [View the public GitHub repository](https://github.com/smadden2-gif/genai-practice)

## The first slice

The page centers on one simulated flow: enter a work question, add optional personal context, explicitly approve whether that context may be considered, and optionally write a separate note to keep private. The result shows what personal context was used and what stayed out. Approval is unchecked by default.

## AI role and transparency

The prototype does not call an AI model. Its response is a fixed browser-side template that includes the work question and, only if the approval box is checked, the personal context. The separate private-note field is never rendered into the result. There is no backend, API key, storage, or network transfer of entered text. This tests the permission and audit interaction, not the quality of an AI-generated answer or a real cross-tool integration.

Note: The assignment suggested Replit, but I did not have access, so I built this with AI-assisted coding (Instinct's agent) and deployed it on GitHub Pages instead.

## Quick tests

These three cases were run against the live prototype after the UI redesign.

### Typical: approved context

**Input.** Work: "I could lead a launch while keeping weekly family commitments."
Approved context: "I have a standing appointment on Thursday evenings."
Private note: "Family health details I will not share here."
Approval was checked.

**Expected.** The work question and approved context appear in the response.
Used lists the approved context. Kept out excludes the private note
without revealing its text.

**Actual. Pass.** The response included the work question and approved
context. Used showed the Thursday commitment. Kept out said the note
stayed out without showing its text.

### Challenge: approval left unchecked

**Input.** Work: "Should I accept a role with frequent travel?"
Personal context: "I may need to care for my parent."
Private note: "Do not use this." Approval was left unchecked.

**Expected.** No personal context is used. Used says none was used.
The private note stays out.

**Actual. Pass.** The no-context response included the work question.
Used said no personal context was used. Kept out described the private
note without exposing it.

### Empty work question

**Input.** Reload the page to start fresh, leave required work input
empty, and submit.

**Expected.** Browser validation blocks submission and the result stays
hidden.

**Actual. Pass.** The browser displayed "Please fill out this field."
The result remained hidden.

## Known limitations

- The response is a fixed template. It does not understand the work question or reason over context. Next: test with people, then evaluate a model using synthetic examples and a clear boundary for approved input.
- The demo does not save decisions or remember permission between visits. Next: define user controls and deletion before considering storage.
- It does not connect to other tools or move context between them. Next: test the permission model before adding an integration, and show exactly what data an integration would send.

## Run locally

Clone the repository and open `index.html` in a browser. No build step or server is required. The page uses a system font stack and no external JavaScript.

```bash
git clone https://github.com/smadden2-gif/genai-practice.git
cd genai-practice
open index.html
```
