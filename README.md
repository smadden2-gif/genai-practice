# Context Bridge

**Context Bridge helps AI use the work and personal context you approve, across the tools where your life is split.**

Your AI starts from zero in each tool, while the context it needs is scattered across work and personal systems. Context Bridge is a permission-first way to carry the right context to a task while keeping the boundary visible.

**[Open Context Bridge on GitHub Pages](https://smadden2-gif.github.io/genai-practice/)**

## Demo Video

[Watch the November walkthrough on Google Drive](https://drive.google.com/file/d/1llttV3kc-WFDBoBNq7vRrlwdBxrOjXrs/view). This 2:25 animated demo shows which details cross the permission boundary in a workshop decision and a calendar handoff. It includes captures from the prototype and selectable English captions. Synthetic narration uses Piper VCTK voice p245 (Irish English, Dublin), based on the [CSTR VCTK Corpus](https://doi.org/10.7488/ds/2645) by Yamagishi, Veaux and MacDonald ([CC BY 4.0](https://creativecommons.org/licenses/by/4.0/)).

## The October prototype

The October prototype introduced the first simulated flow: enter a work question, add optional personal context, explicitly approve whether that context may be considered, and optionally write a separate note to keep private. The result shows what personal context was used and what stayed out. Approval is unchecked by default.

## AI role and transparency

The prototype does not call an AI model. Its response is a fixed browser-side template that includes the work question and, only if the approval box is checked, the personal context. The separate private-note field is never rendered into the result. There is no backend, API key, storage, or network transfer of entered text. This tests the permission and audit interaction, not the quality of an AI-generated answer or a real cross-tool integration.

Note: The assignment suggested Replit, but I did not have access, so I built this with AI-assisted coding (Instinct's agent / Codex) and deployed it on GitHub Pages instead.

## Run the November revision

Open the repository’s [current prototype](index.html) locally, or use the GitHub Pages link above. To run locally, open `index.html` in a browser. The demo runs locally with no installation or build step.

## Try the two flows

### Compare two receipts

1. Under **Fill the form with an example**, choose **Evening workshop**, **Client visit**, or **Training course**. Each button replaces all three text boxes with an editable work question, personal constraint, and private note.
2. Try **Evening workshop** first. Leave approval unchecked, read the preview, and select **Confirm and run**. The receipt contains the workshop question. **Kept out** lists both the personal context and the private note.
3. Check approval and read the preview again. It now includes “I’m unavailable on Thursdays after 5 pm.” Select **Confirm and run** and compare the two receipts. The second includes the scheduling constraint. The reason for it stays private.

Choosing another example clears approval and updates the preview. Earlier receipts stay visible until you reload the page.

### Share busy times between assistants

In **Share busy times**, select **Ask for busy times**. The work assistant offers three fictional busy ranges. **Decline** closes the request. **Approve** shares exactly the previewed ranges with the personal assistant and adds one transfer receipt. Each new request needs its own decision.

## Assignment evaluation

The intended user is someone weighing a work opportunity against a personal commitment. The intended assistant would use a work question and a user-approved constraint to suggest a suitable next step. In this version the form generates a fixed answer, and the calendar handoff uses fictional data. The preview, filtering, approval controls and in-page receipts work in the browser.

The November revision focused on three decisions: show the package before submission, account for every exclusion, and distinguish current permission from the history of earlier runs.

| Case | Synthetic input or action | Success criterion | Observed in Chromium | Result | Implication |
| --- | --- | --- | --- | --- | --- |
| Typical 1 | Approve the Evening workshop constraint and run | Use the scheduling constraint and exclude the private reason | The answer mentioned the Thursday constraint. The private note was absent from the result. | Pass | A user can share availability without sharing the reason. |
| Typical 2 | Ask for the work calendar’s busy times and approve | Deliver the three previewed ranges and create one receipt | The receiving function got only three times. The receipt matched the preview. | Pass | The calendar filter limits the data delivered. |
| Challenge | Ask “Should I travel?” and approve “It depends.” | A helpful answer should identify what information is missing | The template said to check the next step against “It depends.” It gave no clarification. | Partial | Permission worked, but the output did little to help the decision. |
| Invalid | Clear the required work question after a completed run, then submit | Block a new run and preserve history | The browser flagged the missing field. The receipt count stayed at one. | Pass | A missing question cannot generate a receipt. |
| Risk | After an approved run, uncheck permission | Warn that the result reflects earlier permission and preserve its receipt | The stale-result marker appeared, current permission showed as off, and receipt text stayed unchanged. | Pass | A reviewer can distinguish an earlier disclosure from permission for the next run. |

**Before and after.** In both versions, the question was “Lead the workshop?”, personal context was “Thursday appointment”, and the private note was “Synthetic private note”. Approval was off. October reported only “A separate private note was kept out of this result.” November listed both the unapproved personal context and the private note. The difference was visible with the same input.

**What the evidence supports.** These checks show which fields reach a result and what the receipt records. They do not establish that people understand the preview or that the template improves a decision. The ambiguous travel example exposed that limitation. The next priorities are to observe someone predicting what will cross before clicking, then compare how helpful they find the answers with and without an approved constraint.

## November test results

Tested the page markup and interactions in Chromium. All eight checks passed.

| Check | Observed result |
| --- | --- |
| A1 | With both optional fields filled and approval off, Kept out listed both exclusions. |
| A2 | Approving an empty personal field produced “no context entered”. |
| A3 | Unchecking approval marked the result “Permission changed - run again”, showed the current permission and preserved the receipt history. |
| A4 | The confirmed work question and approved personal context matched the receipt text exactly. |
| B1 | All three fictional meetings had a time range, title and private note. |
| B2 | The filter returned only time ranges. The receiving function got only those ranges. The console stayed empty. |
| B3 | Approval started disabled. Declining displayed “I received nothing” without delivering data or adding a receipt. |
| B4 | Two approvals required separate requests and produced two receipt entries. |

All three example buttons filled the three fields, cleared approval, and updated the preview. Switching examples preserved prior receipts. The private note stayed out of each new receipt.

The panels stacked at 375 pixels and appeared side by side at 1200 pixels. The browser recorded zero network requests or script errors during the checks.

## Response to peer feedback

One reviewer recognized the hassle of repeating context across AI tools and asked where AI fit. Other reviewers said the simulation label was clear. They found the unchecked default and the **Used / Kept out** split helpful, so I kept both. Their tests also showed where a user could still be unsure about a particular run.

| Feedback | Decision | What changed and why |
| --- | --- | --- |
| The intended AI role was hard to find. | Clarified; live integration deferred | The calendar example gives the assistant a concrete job: answer “Am I free after three?” using busy times. The answer is simulated so the permission flow can be tested before connecting real tools. |
| A reviewer wanted to see exactly what would be included before submitting. | Acted on | **Review this run** shows the work question and any approved context while the user can still edit them. |
| Checking approval with an empty field produced “not approved.” | Acted on | The result now says “no context entered.” That tells the user why nothing personal was used. |
| Unchecking approval left the earlier result looking current. | Adapted | Of the two suggested fixes, I used the warning “Permission changed - run again.” The page shows the current permission while the receipt preserves what the earlier run actually used. |
| With approval off and both optional fields filled, only the private note appeared under **Kept out**. | Acted on | The audit now names both exclusions without repeating their contents. The before-and-after test above reproduced the original omission and verified the fix. |

## Risk response

Someone asking a work assistant about an evening workshop may want to share when they are available while keeping a family commitment private. Approval starts off, the preview shows exactly what the next run will use, and the private note stays out even when availability is approved. In the revocation test, turning approval off marked the old answer and preserved its receipt. That distinction matters for privacy: changing permission can protect a future run, but it cannot take back a detail already shared. A user could still type a sensitive reason into the shareable field and approve it. The preview gives them a chance to catch that mistake; the prototype cannot tell which details they meant to keep private.

## AI-use disclosure

I used Codex and Instinct to make changes to the prototype code. I also used AI to help make the demo video and experimented with animation and synthetic voice models. The earlier cut followed the form from field to field. I wanted viewers to see why the sharing decision mattered, so I asked for a simpler story about the Thursday workshop and chose an Irish male voice for the final cut.

Two excerpts from my prompts show how I used the tools:

- “Never edit or delete the receipt history of what was actually sent” guided the code change that marks an earlier result when permission changes and keeps its receipt intact.
- “It doesn't necessarily all have to be the walkthrough of the demo” guided the video edit. The final cut uses animated cards to show availability crossing the boundary while the private dinner plans stay behind.

The browser tests checked the preview, exclusions and receipt history with fictional inputs. The assistants in the product use fixed responses. These tests show what the demo shares, but they do not establish how a real assistant would handle sensitive information.

For a final product, I need to do more research on context sharing and the Agent2Agent (A2A) protocol. One question I want to test is whether a receiving assistant can answer “Am I free after three?” using only busy times, without private meeting titles or notes appearing in messages or logs. The current demo simulates that handoff locally.
