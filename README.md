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

## Run the November revision

Check out `revision-nov19` and open `index.html` in a browser. The demo runs locally with no installation or build step.

## Try the two flows

1. Enter a work question and optional personal context. Review the preview, then choose **Confirm and run**. Change permission to see how the next run differs. Earlier receipts stay visible until the page reloads.
2. In **Share busy times**, select **Ask for busy times**. The work assistant offers three fictional busy ranges. **Decline** closes the request. **Approve** shares exactly the previewed ranges with the personal assistant and adds one transfer receipt. Each new request needs its own decision.

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

The panels stacked at 375 pixels and appeared side by side at 1200 pixels. The browser recorded zero network requests or script errors during the checks.

## Response to peer feedback

TODO

## Risk response

TODO

## AI-use disclosure

TODO
