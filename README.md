# ehr-utils-project

You have been granted access to a repository: https://github.com/biostat821-2025/ehr-utils-{username}. It will eventually contain six issues with instructions for each of the six phases of the project. You will progress through the phases one-at-a-time, in order. Each phase is completed by generating a pull request (PR), getting it approved, and merging it into the repository's `main` branch. Your work in each phase will build upon the previous one - expanding and improving your library!

## General process, per phase

1. Create branch "phase-X".
2. Create PR "Phase X".
   * base: main ← compare: phase-X
   * All of your PRs will have `main` as the "base" - this is where you want the changes to go.
3. Make changes: edit, `git add`, `git commit`, etc.
   * You've already configured VS Code to perform all of the linter checks. (Right?!)
     * If you're stumped, ask for help immediately via Slack, email, or office hours.
   * Run tests with `pytest tests/`.
     * If you're stumped, ask for help immediately via Slack, email, or office hours.
   * After pushing to the GitHub "phase-X" branch, confirm that the PR checks pass.
     * If they do not, see the associated logs. Also look into getting your local development environment fixed.
4. [Request a PR review.](https://docs.github.com/en/pull-requests/collaborating-with-pull-requests/proposing-changes-to-your-work-with-pull-requests/requesting-a-pull-request-review#requesting-reviews-from-collaborators-and-organization-members)
5. Wait for review (≤ 3 days).
   * If you have waited more than 3 days, ping Patrick relentlessly until you get a review.
6. If approved, you may merge the PR and move on to the next phase 🎉; otherwise...
7. Read and act upon all review comments.
   * Edit, `git add`, `git commit`, etc. 
8. [Re-request a PR review.](https://docs.github.com/en/pull-requests/collaborating-with-pull-requests/proposing-changes-to-your-work-with-pull-requests/requesting-a-pull-request-review#requesting-reviews-from-collaborators-and-organization-members)
   * This is how we know that it's our turn. Until you do this, we will assume that you are still working on it. Reach out if you're not sure whose turn it is.
9. GOTO 5

We will repeat this as many times as necessary to get approval.

## Important notes

* Only one PR can be under review at any given time. You may work ahead, but we will not review phase X+1 until phase X is approved and merged, _and_ you have rebased the `phase-X+1` branch on the `main` branch (see detailed phase transition process below).
* Corrollary: respond to PR review comments quickly! The phase X+1 review is blocked until your phase X PR has been approved (and merged).
* After changes are requested by a reviewer, make sure to address _all_ comments before re-requesting a review.
* Code reviews will often only comment on one instance of an issue. For example, if you receive the comment "use snake_case for Python variable names", you should scan all of your code for other places this comment may apply.

## Phase transition process breakdown

* On GitHub, Merge phase X PR (into GitHub's `main`).
* In local terminal, switch to local `main` branch.
  ```bash
  git switch main
  ```
* Update local `main` branch.
  ```bash
  git pull
  ```
* Switch to feature branch.
  ```bash
  git switch phase-X+1
  ```
* Rebase feature branch onto `main` branch.
  ```bash
  git rebase main
  ```
* Push local feature branch to GitHub.
  ```bash
  git push
  ```

## Schedule

| date | due |
| ---- | --------|
| 1/29 | Phase 1 PR review requested |
| 2/12 | Phase 1 PR merged <br/>Phase 2 PR review requested |
| 2/26 | Phase 2 PR merged <br/>Phase 3 PR review requested |
| 3/05 | Phase 3 PR merged <br/>Phase 4 PR review requested |
| 3/26 | Phase 4 PR merged <br/>Phase 5 PR review requested |
| 4/09 | Phase 5 PR merged <br/>Phase 6 PR review requested |
| 4/16 | Phase 6 PR merged |

## Scoring

There are 12 equally-weighted parts corresponding to rows above.

"Review requested" requires that the automated checks pass.

"Merged" requires approval by instructor(s).

If these are completed on-time, full credit will be awarded.
If late, each is 20% off per day late:

| days late | penalty |
| --------- | ------- |
| 0-1       | -20%    |
| 1-2       | -40%    |
| 2-3       | -60%    |
| 3-4       | -80%    |
| 4+        | -100%   |

