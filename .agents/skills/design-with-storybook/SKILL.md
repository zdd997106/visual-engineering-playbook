---
name: design-with-storybook
description: Explicitly invoked workflow for designers to preview and refine production UI in Storybook, save accepted changes locally, and confirm delivery for engineering review.
---

# Design with Storybook

Start only when the designer explicitly invokes `$design-with-storybook` in their prompt or selects this skill in the tool's prompt picker to use it for the task. On clients without a picker, an explicit request to use this named skill also counts. Installing, attaching, discussing, or editing the skill does not start it. A matching UI or Storybook request alone does not activate it. Once invoked, follow ordinary feedback throughout that design task. A new task or new chat needs a fresh invocation.

Reply in the user's language, including questions, preview instructions, progress updates, and handoff messages. Follow the repository's conventions for commit messages and PRs. The designer describes what feels wrong, tries the result, and decides when it looks right. Manage the engineering workflow yourself.

## Start from the product screen

Find the product repository, its contribution instructions, working tree, index, Storybook setup, and the actual entry point for the requested screen. Use labels or navigation paths to resolve ambiguity. For a new design task, branch from the latest remote base specified by the project. For a returning task, recover the existing branch, preview, and last accepted change from local task notes. Preserve unrelated work.

A request to discuss an idea calls for a discussion. A request to see the current screen authorizes preparing a preview. If the target and intent are clear, proceed directly. When two screens fit the description, ask which one the designer uses.

## Give the designer a working preview

- Render the real product components. Use an existing story if it fits. Otherwise compose the page or flow with the project's router, providers, and mocked data and responses as needed. A retry or a cross-page transition should work when clicked. Keep requests inside the mock environment so the designer can try the flow without a running backend or a real write.
- Keep temporary stories, mocks, and brief boundary notes local to each task. Record where the real UI lives, which existing code or tests establish the relevant business rules, and what is still uncertain. Rebuild an obsolete task preview when needed. Reuse the repository's Storybook setup without requiring shared configuration changes for adoption.
- Choose a local filename or configuration Storybook can load. Verify local-only files are untracked and excluded from delivery. Account for tracked files and any build or publication process that discovers stories automatically.
- Verify the scenario and give the designer a direct link with a short action to try. Say which response is simulated. If you could only build the story, report exactly that and leave visual acceptance to the designer.

## Let feedback determine the next action

After invocation, interpret feedback in the context of what the designer just saw. They can accept a design in their own words.

After each edit, including follow-up tweaks in the same round, do a quick business-rule check before handing back the preview or committing. Compare the diff for that edit with the relevant rules and their source references in the task notes. For a label or spacing change, checking the affected lines and their context is usually enough. If the edit touches an event handler or a condition, trace that path in the production code and use a focused existing test when useful. For example, changing an invite button's pending label must preserve the guard that prevents a second submission. A successful mock response alone cannot verify that guard.

If your edit changed a rule, correct your own change and repeat the check. If the intended behavior is unclear or the design requires changing the rule, pause that part before acceptance or commit and explain what an engineer needs to resolve. A passing check requires no extra action from the designer.

| What the designer means | What to do |
| --- | --- |
| They want another adjustment to the current result. | Edit the production presentation layer, refresh the same story, and invite them to try the change. Keep the round open. |
| They have tried the changed UI and clearly accept it, for example, “That looks right” or “Keep this one. Let's look at the error screen.” | Automatically create one **local commit** for the accepted change, then continue with any clear next request. Briefly say what you saved. Do not ask again whether to commit. |
| They switch screens while an edited result is awaiting a decision. | Ask one question in their language, tied to what changed, such as “Before we switch, are you happy with the new spacing on the invite form, or would you like to come back to it?” Wait for the answer before committing that change. If deferred, keep it distinguishable from the next round. |
| They approve exploring an idea, identify the right screen, or say that the preview opens. | Continue the requested work. These signals refer to the idea or preview, not acceptance of a product edit. A preview-only round has nothing to commit. |
| They want the finished work sent for engineering review. | Save any clearly accepted final change and prepare the handoff proposal below. Wait for the designer to approve it before renaming the branch, pushing, or opening or updating a PR. |

For partial feedback such as “The label is right, but the spacing still jumps,” continue the same round until the remaining issue is resolved unless the designer asks to save a separable part. Honor requests to hold, revisit, or skip a commit. On a retry or repeated acceptance, check the working diff and branch history first so the same change is saved once.

Preserve existing permissions, API contracts, data writes, and business rules. Small presentation refactors are in scope. If a request crosses that boundary, explain the specific dependency and what an engineer needs to resolve. Shared components and global tokens are within the designer's remit. Identify representative consumers and show the impact before treating a global change as accepted.

## Save an accepted round

1. Review the diff and inspect the index **before every staging operation**. Stage only this round's accepted production changes and any shared stories the project maintains. Keep task previews, notes, personal skill installation files, and unrelated edits out of the commit.
2. Respect repository and user staging rules. If existing staged work requires approval, leave the index untouched and ask the required yes/no question naming the exact paths or hunks. Design acceptance does not override that rule. An approval limited to staging authorizes only the named staging action. Preserve pre-existing staged work and never include it in a design commit.
3. With a clear index and a scoped diff, stage the accepted changes, inspect the staged diff, commit using the project's conventions, and verify what was committed. If checks fail, keep the work available and report the actual blocker. Give the designer the visible result in one sentence.

## Hand off for engineering review

1. **Prepare locally.** On a handoff request, update from the base and organize local commits as needed (rebase, squash, reword). Preserve shared history. After integration, compare the final production diff against the relevant business rules in the task notes. If resolving a conflict changes the accepted experience, return to the designer for visual acceptance. Route conflicts about business behavior to engineering. Run relevant checks and inspect the diff and outgoing commits for local-only material.
2. **Ask once for the proposed delivery.** Summarize what will be sent and the check results. Name the current and proposed branch (or say the current name will be kept), the destination repository and base branch, and the title of the new PR or the existing PR to update. Ask whether to carry out those specific actions now. Even an initial request to rename, push, and open a PR starts this confirmation step. Wait for a separate, explicit affirmative reply. While waiting, keep the branch name unchanged, the commits local, and any draft or existing PR untouched.
3. **Carry out what was approved.** A single affirmative reply covers the actions shown together. If the designer defers or approves only part, respect that scope. Pushing to an open PR's head branch updates that PR too, so include that effect in the proposal. If the changes to deliver, destination, branch name, or PR action changes materially, present the revised proposal for approval. This gate also applies to later pushes to an existing PR. Before retrying an approved action, check the current branch, remote, and PR so a partial success is not repeated. Describe the accepted experience, actual check results, and technical review points in the PR. Include visual evidence when requested. Return the link and leave review and merge to engineering.
