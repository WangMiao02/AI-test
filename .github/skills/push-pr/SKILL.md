---
name: push-pr
description: 'Use when pushing a branch, opening a pull request, preparing a PR body, or checking whether local changes are ready to publish. Trigger for 推PR, 发PR, push branch, create PR, open pull request.'
argument-hint: 'branch name, base branch, and optional PR summary'
user-invocable: true
---

# Push PR

Use this skill when the task is to publish local changes and turn them into a pull request.

## When to Use

- Push the current branch to remote
- Create or update a pull request
- Prepare a PR title and body
- Check whether the branch is safe to publish
- Handle common blockers such as no remote, no upstream branch, or missing commits

## Inputs to Collect

- Current branch name
- Target remote, usually origin
- Base branch, usually main or master
- Short change summary for the PR title and body
- Whether there are required checks to run before publishing

## Procedure

1. Confirm repository state.
   - Run git status --short --branch
   - Run git remote -v
   - Run git branch --show-current
2. Validate before publishing.
   - Prefer the narrowest test, build, or lint command available for the touched scope
   - If no executable validation exists, review the staged diff and summarize risk clearly
3. Prepare commits.
   - Stage only intended files
   - Use a focused commit message
   - If there are no commits yet, create the first commit before pushing
4. Push the branch.
   - If upstream is missing, run git push -u origin <branch>
   - If upstream exists, run git push
5. Prepare the pull request.
   - Write a concise title: <area>: <change>
   - Use the template in [assets/pr-body-template.md](./assets/pr-body-template.md)
   - Fill in summary, validation, and risks
6. Open or share the PR link.
   - If GitHub CLI is available, use gh pr create
   - Otherwise construct the compare URL manually:
     https://github.com/<owner>/<repo>/compare/<base>...<branch>?expand=1

## Safety Rules

- Do not push if the user explicitly asked to hold changes locally
- Do not publish secrets, tokens, credentials, or private data
- Do not use git push --force unless the user explicitly asks for it
- If authentication is required, let the user type secrets directly in the terminal

## Common Recovery Paths

### No remote configured

1. Ask for the remote URL
2. Run git remote add origin <url>
3. Retry the push

### No commits yet

1. Configure local Git identity if needed
2. Stage intended files
3. Create the first commit
4. Push with upstream

### Permission or auth failure

1. Check which GitHub account is being used
2. Clear or bypass cached HTTPS credentials if needed
3. Re-run push and let the user type a valid PAT or use SSH

## Output Checklist

- Branch pushed successfully
- Remote tracking branch configured
- PR title drafted
- PR body drafted from template
- Validation steps listed
- Remaining risks or blockers called out explicitly
