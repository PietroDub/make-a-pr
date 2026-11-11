# Open or update the pull request

Run this after the body is filled, including the trailer from [coauthor.md](coauthor.md), and only after [ask.md](ask.md) says the user already chose GitHub. A missing choice means the body is in chat, the question has been asked, and this file stays unread.

Finish when the chat contains the pull request URL. The body on GitHub matches the fenced body, without the fence.

Use `gh` for every GitHub call. The account is whoever `gh` is already logged in as. Do not change git config. Do not force-push. Do not skip hooks. Do not merge the pull request.

## Before the write

Run these in parallel:

- `git status`
- `git diff` and `git diff --staged`
- `git branch -vv`
- `git log --oneline -15`
- `gh pr list --head <current-branch> --json number,url,title,baseRefName`

The base is the open pull request's base when one exists, otherwise the repository default. Name that base in the template's notes section when you assumed it.

## A pull request already exists for this branch

Update that pull request in place:

```bash
gh pr edit <number> --title "<title>" --body "$(cat <<'EOF'
<filled body, trailer included>
EOF
)"
```

Leave the base branch as it is. Then `gh pr view <number> --json url -q .url` and return that URL.

## No pull request yet

Push the branch, then open the pull request against the base:

```bash
git push -u origin HEAD

gh pr create --base <base> --title "<title>" --body "$(cat <<'EOF'
<filled body, trailer included>
EOF
)"
```

Return the URL `gh pr create` prints.

## Title

One line, the change a reviewer sees first. Lead with the task id when the body has one (`[#1gaax8j7rwb]`). Keep code identifiers as they appear in the repo. The title is not the first line of the template (that line is the project name).
