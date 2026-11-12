# Branch sync

Run this on every body, before the checklist is filled. A clean working tree is a separate fact. A comparison already printed in the chat is stale. Run it again.

The base is the open pull request's base, or the repository default when there is no pull request yet. Use the same base the rest of the body uses.

```bash
git fetch origin <base>
git rev-list --left-right --count origin/<base>...HEAD
```

The command prints two integers separated by a tab: `behind` then `ahead`. `behind` is the number of commits on `origin/<base>` that are not in `HEAD`. `ahead` is the number of commits on `HEAD` that are not in `origin/<base>`.

Check the branch box only when `behind` is `0`. Any larger `behind` leaves the box empty. `ahead` does not decide the box.

Write the count in the notes section. On the default template that section is **Informações adicionais**, and the sentence is:

`A branch está <behind> commits atrás de <base>.`

Use the integer from the command. Zero stays zero. Do not round and do not omit the sentence when the box is checked.

When `git fetch` fails, or `origin/<base>` does not exist, leave the box empty and write which command failed. Do not invent a count. Do not merge, rebase, or reset to make the count zero.

A custom template gets the same fact in its notes heading, in that template's language: the integer and the base name.
