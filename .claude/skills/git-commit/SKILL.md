---
name: git-commit
description: Analyze current Git changes, select one coherent set of files, and create exactly one Conventional Commit without pushing.
---

# Git Commit

Create one commit from one logically related set of current repository changes. Never push.

## Procedure

1. **Inspect the working tree.** Run `git status --short --branch`, then review the unstaged diff (`git diff`), staged diff (`git diff --cached`), and relevant untracked files. Read enough surrounding code or project context to understand what each changed file does. Do not print or include secrets; if a diff appears to contain credentials or other sensitive data, stop and report it.
2. **Choose one coherent change.** Group files by the single behavior, fix, or purpose they implement. Prefer the smallest complete and internally consistent group; do not combine unrelated changes merely to empty the working tree. If there are several groups, choose one group and leave the others untouched. Briefly state the selected files and the reason before committing.
3. **Respect existing staging.** Do not unstage, reset, discard, or overwrite changes. If the staged changes form the selected coherent group, use that staged content. If the index contains unrelated work, do not accidentally include it: select only the chosen files when safe. If staged and unstaged edits overlap on the same path, or a safe selection cannot be made without altering the user's staged state, stop and ask the user how to proceed.
4. **Stage only the selected unstaged/untracked paths**, if needed, using explicit paths (never `git add .` or `git add -A`). Review `git diff --cached` and `git status` after staging. Confirm the staged content is exactly the chosen change and contains no unrelated or sensitive material. If not, do not commit; report the issue and ask the user.
5. **Write a Conventional Commit message** in the form `<type>(<scope>): <imperative summary>`; omit the scope when not useful. Choose an accurate type such as `feat`, `fix`, `docs`, `refactor`, `test`, `build`, `ci`, `perf`, or `chore`. Keep the summary concise, imperative, and without a trailing period. Do not claim behavior that the diff does not implement.
6. **Create exactly one commit** with `git commit -m "<message>"`. Do not amend or create a second commit as part of this task. Never use `--amend` or `--no-verify`, and do not bypass hooks. If the commit fails, report the failure and do not retry with bypass flags.
7. **Verify and report.** Obtain the resulting full hash and message (for example, with `git show -s --format=%H%n%s HEAD`) and the committed paths (for example, `git diff-tree --no-commit-id --name-only -r HEAD`). Ensure the commit is the one just created and paths match the selected set. Return:
   - commit hash
   - commit message
   - committed files

If there are no changes, do not create an empty commit; report that there is nothing to commit. If no single safe, coherent set can be identified, ask the user instead of guessing.
