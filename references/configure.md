# Save a project profile

Use this when the user is teaching a pull request format. The bundled files `references/template.md` and `references/example.md` stay untouched. A profile is a new folder beside them.

Finish when the folder exists on disk and the user has the path, the match names, and the confirmation that the next pull request for this repo uses this template.

## What the user is giving you

The format is the file they attached or the template they pasted. Save that text as the template. Keep their labels, language, order, and checkboxes. Leave their wording as they wrote it.

A filled pull request they sent with the format is the example. Save it only when they sent one.

They may also say the language, the task-tracker shape, which heading holds reviewer notes, and extra names the repo goes by. Those belong in `profile.md`.

## Where it goes

Write under `references/projects/<slug>/` next to the `SKILL.md` you are following.

Find that skill on disk in this order and use the first copy that exists:

1. The open workspace, when its root `SKILL.md` has `name: make-a-pr`
2. `~/.cursor/skills/make-a-pr`
3. `~/.agents/skills/make-a-pr`
4. `<current-repo>/.cursor/skills/make-a-pr`
5. `<current-repo>/.agents/skills/make-a-pr`

A symlink at `~/.cursor/skills/make-a-pr` that points at the skill repo keeps the saved profile in that repo. The same search order is how a later run finds the profile.

`<slug>` is the repo folder name, lowercase, with spaces turned into hyphens. When the user gives a project name that differs from the folder, use their name as an extra `Match` line and keep the folder slug as the repo folder.

Create the folder if it is missing. When that slug already exists, replace `template.md` with the new format and refresh `profile.md`. Replace `example.md` only when they sent a new filled example. Leave an existing `example.md` in place when they sent a template only.

Write the files. Leave git commit and push for when the user asks.

## profile.md

```markdown
# <Project name>

Match:
- <repo folder name>
- <package name, when the repo has one>
- <origin repo name>
- <any other name the user said this project uses>

Language: <language of the template>

Task line: <the placeholder line copied from their template, or none>

Notes heading: <the heading in their template where extra reviewer context goes, or none>

Voice: <example.md in this folder, or the template headings when they sent no filled example>
```

`Notes heading` is the section a reviewer reads for contract changes, an assumed base branch, a missing task id, and why a checklist box is open. Use the heading they name. When they name none, use the last free-text section in their template. When the template has no free-text section, write `none`.

`Task line` is `none` when their template has no ticket placeholder. The ClickUp line from the default template stays in the default template.

## After the files are saved

Tell the user, in a short note outside any pull request body:

- the project name and the slug
- the full path of the profile folder
- the `Match` names
- that the next pull request for a matching repo fills this template
- that `references/template.md` is still the default for every other repo

When they also asked for the body, write it next by following [write-custom.md](write-custom.md) with this profile.

## Forget a project

When the user says to drop a project's format, delete `references/projects/<slug>/` and tell them the default template applies to that repo again.
