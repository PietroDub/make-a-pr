# How the suite prints a count

The count in the body is a number that already appeared in this chat or in the terminal. A look at the project names the command. It does not supply the count.

Find the command the same way as the default section of `SKILL.md`: a command and result already in the chat or terminal, then `package.json` scripts, `pyproject.toml`, `go.mod`, `Cargo.toml`, `composer.json`, `Makefile`, `justfile`, or the CI workflow.

Leave the suite unrun when nobody has run it. Write that it was not run in this session and name the command. Invent no count and no passing result.

When a run is already on screen, copy the totals from the line below. Ignore timings. A summary that says failed contributes its failed count. "Todos passando" belongs in the sentence only when the summary itself has no failures.

| Runner | The line that holds the count |
| --- | --- |
| Jest (`npm test`, `npm run test`) | `Test Suites:` and `Tests:` (`28 passed, 28 total` and `232 passed, 232 total`) |
| Vitest | `Test Files` and `Tests` (`3 passed (3)`, `12 passed (12)`) |
| pytest | the last line (`12 passed`, or `10 passed, 2 failed`) |
| Cargo | `test result:` (`12 passed; 0 failed`) |
| Maven Surefire | `Tests run: 12, Failures: 0, Errors: 0, Skipped: 0` |
| PHPUnit | `OK (12 tests, 30 assertions)` or `Tests: 12` |
| `go test` | one `ok` or `FAIL` per package. That line has no test total. Report the packages. Do not turn them into a test count. |

On the default template the sentence stays in the example's shape: the kind of test, the command, then those counts (`28 suites, 232 testes, todos passando.`). A custom template says the same fact in its language.

A Makefile or justfile target is the command. The count still comes from whichever runner that target printed.
