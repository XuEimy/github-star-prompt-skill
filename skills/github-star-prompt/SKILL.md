---
name: github-star-prompt
description: Implement a dismissible GitHub star prompt in an app, with appropriate timing, persistence, and a user-initiated GitHub action. Use when asked to add or improve a prompt encouraging users to star a repository.
---

# GitHub star prompt

Implement an unobtrusive, optional prompt that links to the repository chosen by the user. Inspect the app's UI, storage, and external-link conventions first; adapt to its supported clients and avoid adding a dependency for a simple prompt.

## Behavior

- Trigger only after a meaningful successful interaction, not on first launch. Choose a threshold suited to the product; a starting example is after the sixth successful message. Count only successful actions.
- Wait for a focused, idle window before displaying the prompt. Reset the idle timer on user activity; do not show it while an embedded view has focus. A 30-second idle period is an example, not a fixed requirement. Cancel timers and listeners on teardown.
- Limit prompts to once per session and persist dismissal across sessions. Keep the count and dismissal in separate keys so concurrent tabs cannot overwrite a dismissal. Recheck dismissal after asynchronous operations and immediately before showing. If persistence fails, do not show a prompt that cannot remember dismissal.
- Provide an accessible close action and a clear action to open the selected repository on GitHub. Do not star a repository without an explicit user action. Prefer opening GitHub in a browser; if an existing authenticated integration supports starring in place, use it only on click and make clear which account will act. A server account may not belong to the current user.
- If checking whether a repository is already starred, use the relevant environment and existing authorized integration. A failed check should not be treated as a permanent dismissal. Do not require login just to see the prompt.
- Handle external-link failures and browser user-activation restrictions: if an asynchronous attempt expires the click gesture, offer a fresh click to open GitHub. Respect reduced-motion preferences for any optional celebration.

## Verification

Exercise the successful interaction threshold, idle and focus behavior, dismissal across sessions and concurrent tabs, storage failures, and any asynchronous check or action failure. Verify that the prompt is dismissible and its GitHub action only runs after a click. Test the relevant product entry points and clients using the project's normal validation workflow.
