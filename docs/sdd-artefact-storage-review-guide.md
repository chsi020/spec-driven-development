# Spec Driven Development

**SDD Artefact Storage & Review Guide**

*How spec and plan documents are stored, reviewed, and linked in GitHub and Linear*

| | |
| :--- | :--- |
| **Version** | 1.0 |
| **Date** | 24 March 2026 |
| **Audience** | Product Managers, Developers |

---

## 1. The Problem This Guide Solves

The SDD workflow produces two critical artefacts — the **spec document** and the **implementation plan** — that must be reviewed and approved by both the product manager and developer before implementation begins. Without a clear, consistent home for these documents, teams run into three recurring problems.

| Problem | Impact on SDD |
| :--- | :--- |
| Docs live in chat history | Context is lost between sessions; the AI agent cannot reference an approved spec in a new session |
| No formal approval record | The gate sign-off is a verbal or Slack agreement — invisible to future developers and auditors |
| PM cannot access the docs | Spec docs stored only in the codebase exclude the PM from review unless a separate copy is maintained |

> **The core tension:** Spec and plan docs need to be authoritative enough that an AI agent reads them as the source of truth, but collaborative enough that a product manager — who may not be comfortable with Git — can actually review and comment on them.

---

## 2. Recommended Approach

Store spec and plan docs in the GitHub repository using the **docs-as-code** pattern, with GitHub Pull Requests as the review and approval mechanism. Linear is updated with links to the approved artefacts after each gate.

### 2.1 Where Docs Live in the Repo

All SDD artefacts are stored under `docs/specs/`, named by their Linear issue ID:

```
docs/
  specs/
    README.md                           ← explains folder structure and workflow
    DEV-123-user-dashboard-spec.md      ← approved spec
    DEV-123-user-dashboard-plan.md      ← approved plan
    FEAT-456-search-refactor-spec.md    ← in review
```

The Linear issue ID in the filename creates a permanent, human-readable link between the artefact and the work item. The `README.md` in the folder explains the structure to any future developer or AI agent briefed to read the specs.

### 2.2 File Naming Convention

| Artefact | File name pattern | Example |
| :--- | :--- | :--- |
| Spec document | `{ISSUE-ID}-{slug}-spec.md` | `DEV-123-dashboard-spec.md` |
| Implementation plan | `{ISSUE-ID}-{slug}-plan.md` | `DEV-123-dashboard-plan.md` |

### 2.3 Markdown Frontmatter

Every spec and plan doc must include a YAML frontmatter block at the top. This makes the approval status machine-readable for both humans and AI agents:

```yaml
---
linear_issue:    DEV-123
title:           User dashboard
status:          approved          # draft | in-review | approved
type:            spec              # spec | plan
author:          @dev-handle
pm_approved_by:  @pm-handle
approved_date:   2026-03-24
---
```

> **Why frontmatter matters for AI agents:** When an AI agent is briefed to read the spec before implementation, it can parse the frontmatter to confirm the document is approved before proceeding. A draft spec should never be used as the basis for code generation.

---

## 3. The Review Workflow — PR as Gate

GitHub Pull Requests are the review and approval mechanism for SDD gates. Each spec and plan doc is introduced via its own PR, which is merged only after both the PM and developer have approved it. **The PR approval is the gate sign-off.**

### 3.1 Step-by-Step for the Spec Gate (Step 4)

1. Developer opens a draft PR containing only the spec doc
2. PR description includes a summary of the feature and tags the Linear issue (e.g. `Refs DEV-123`)
3. Developer marks the PR as ready for review and manually requests the PM as a reviewer
4. PM reviews the spec using GitHub's rich diff view, which renders Markdown — no Git knowledge required
5. PM and developer leave inline comments on specific lines; all discussion is captured in the PR thread
6. PM explicitly approves the PR — this is the Gate 4 sign-off, with a permanent audit trail
7. Developer merges the spec PR and opens a second PR for the plan

> **For product managers — you don't need to know Git:** GitHub's web interface renders Markdown in a readable format. You can review the spec, leave comments on any line, and click Approve — all in the browser, without installing anything. Ask your developer to send you the PR link directly.

### 3.2 GitHub's Rich Diff View

When reviewing a PR that modifies a Markdown file, GitHub defaults to showing raw Markdown source. Switch to the **rich diff view** by clicking the icon at the top-right of the file diff. This renders the document as formatted text, making it far easier for a PM to read and review.

### 3.3 CODEOWNERS for Automatic Reviewer Notification

Add a `CODEOWNERS` file to your repository so that any change to `docs/specs/` automatically requests the right reviewers without anyone having to remember:

```
# .github/CODEOWNERS
docs/specs/  @your-org/product-managers @your-org/developers
```

This ensures the PM is automatically notified of every new spec PR. The developer still marks the PR as ready for review manually — use Draft PRs to avoid notifying reviewers before the spec is ready.

---

## 4. Linking Artefacts to Linear

After each gate is passed, the artefact is linked back to the Linear issue so the PM and developer have a single place to find everything related to the feature.

### 4.1 Automatic Linking via Magic Words

Include the Linear issue ID in the PR description using Linear's magic words syntax. This creates a bi-directional link between the PR and the issue:

```
## Spec: User dashboard

Refs DEV-123

This PR introduces the approved spec for the user dashboard feature.
Review against the acceptance criteria in the Linear issue.
```

Linear will automatically show the spec PR on the DEV-123 issue. When the PR is merged, the link persists — giving future developers the full history of spec decisions.

### 4.2 Manual Attachment Link

Once the spec PR is merged, the developer posts the direct GitHub URL of the spec file as an attachment link on the Linear issue. This takes about ten seconds and creates a one-click path from Linear to the approved artefact:

| Artefact | Link to add on Linear issue |
| :--- | :--- |
| Spec document | `github.com/org/repo/blob/main/docs/specs/DEV-123-...-spec.md` |
| Implementation plan | `github.com/org/repo/blob/main/docs/specs/DEV-123-...-plan.md` |
| Spec PR | `github.com/org/repo/pull/{spec-pr-number}` |

---

## 5. What This Gives You

| Need | How it's met |
| :--- | :--- |
| PM can read and comment | GitHub PR rich diff renders Markdown — no Git knowledge required |
| Formal approval gate | PR approval = gate sign-off, with full audit trail |
| Automatic reviewer notification | CODEOWNERS on `docs/specs/` pings the right people on every PR |
| Linked to Linear | `Refs` magic word in PR + manual attachment link on the issue |
| AI agent can read the spec | Markdown in the repo, frontmatter confirms approval status |
| Full version history | Git tracks every change to the spec with author, date, and reason |
| No additional tooling | Everything is GitHub — no Notion, Confluence, or third-party sync required |

---

## 6. Common Pitfalls to Avoid

| Pitfall | How to avoid it |
| :--- | :--- |
| Merging a spec before PM approval | Set branch protection rules requiring at least one approval from the `product-managers` team before merge is allowed |
| Updating the spec after approval without a new PR | Any change to an approved spec must go through a new PR and be re-approved. Update the frontmatter `status` back to `in-review` |
| Plan references a stale spec | The plan PR should always be opened from a branch that includes the merged spec. Never create the plan before the spec is merged |
| Forgetting to link artefacts on Linear | Add the manual attachment link step to your team's definition of done for each gate |
| AI agent reads a draft spec | Always brief the agent to check the frontmatter `status` field before using the spec as a source of truth |

---

## 7. Quick Reference Checklist

### Spec Gate (after Step 3)

- [ ] Spec doc created at `docs/specs/{ISSUE-ID}-{slug}-spec.md`
- [ ] Frontmatter present with `status: in-review`
- [ ] Draft PR opened with `Refs {ISSUE-ID}` in description
- [ ] PM requested as reviewer
- [ ] PM and developer have reviewed and left any comments
- [ ] PM has approved the PR
- [ ] Spec PR merged; frontmatter updated to `status: approved`
- [ ] Attachment link added to Linear issue

### Plan Gate (after Step 5)

- [ ] Plan doc created at `docs/specs/{ISSUE-ID}-{slug}-plan.md`
- [ ] Plan references the approved spec by filename
- [ ] Frontmatter present with `status: in-review`
- [ ] Draft PR opened with `Refs {ISSUE-ID}` in description
- [ ] Developer has reviewed and approved the PR
- [ ] Plan PR merged; frontmatter updated to `status: approved`
- [ ] Attachment link added to Linear issue

---

> **Remember:** The spec is the most important artefact in SDD. A bad spec cascades problems through every subsequent step. The review process described in this guide exists to protect the quality of that spec — don't rush or skip any part of it.

---

*Spec Driven Development transforms AI from an unpredictable tool into a disciplined collaborator — one that amplifies human judgement rather than replacing it.*
