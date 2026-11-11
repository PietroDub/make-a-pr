# Co-author trailer

Every pull request body this skill writes ends with a blank line and exactly one `Co-authored-by` trailer for the host that is running the skill. The host is the application, not the model name. A Claude model inside Cursor is Cursor. Cursor's model inside Claude Code is Claude Code.

The trailer sits after the filled template. It is not a new template section. The same line is the last line of the fenced block in chat and the last line of the body sent to GitHub.

```text
<filled template>

Co-authored-by: Name <email>
```

One blank line before the trailer. No second copy when that exact line is already the last line.

| Host | Trailer |
| --- | --- |
| Cursor, including a Cursor cloud agent | `Co-authored-by: Cursor <cursoragent@cursor.com>` |
| Claude Code | `Co-authored-by: Claude <noreply@anthropic.com>` |

Decide the host in this order:

1. Claude Code when the session is Claude Code or the environment has `CLAUDECODE` or `CLAUDE_CODE_ENTRYPOINT`.
2. Cursor when the session is Cursor, or the environment has `CURSOR_AGENT` or `CURSOR_TRACE_ID`.
3. Another host when you know the `Co-authored-by` line that product already writes in its own commits. Use that line, name and email unchanged.

When none of the three match, use `Co-authored-by: Cursor <cursoragent@cursor.com>` if the skill was loaded from Cursor, and `Co-authored-by: Claude <noreply@anthropic.com>` if it was loaded from Claude. Record the host you chose in the template's notes section, in that template's language.

A commit you create while handling this request uses the same trailer: one blank line, then the line, at the end of the commit message. Leave commits that are already on the branch as they are. Do not amend, rebase, or rewrite them to insert the trailer.

GitHub records the trailer as a co-author when it is on the commit that lands in the default branch. A squash merge uses the pull request body as the commit message, so the line at the end of the body is what gets recorded. A merge commit keeps the trailers that are already on the branch commits, plus this line on any commit you create in this request.
