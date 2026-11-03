# Development workflow

These rules apply to the whole repository.

## Repository roles and safety

- Treat this checkout as the editable source repository.
- The canonical Skill source is `skills/directing-stickman-videos/`.
- The active Codex installation is expected at `~/.codex/skills/directing-stickman-videos` and should be a directory symlink or junction to the canonical Skill source.
- Never develop, commit, or run broad cleanup commands inside an installed copy, plugin cache, `.codex/skills/.system`, or another system-managed directory.
- Before replacing an installed copy, compare it with the source while ignoring generated files and line-ending-only changes. Preserve any meaningful user changes in a dated backup or migrate them into a branch.
- Keep generated dependencies, render caches, previews, credentials, and `.env*` files out of Git.

## Git remotes and branches

- `origin` must be `https://github.com/Nathanphop/stickman-video-director.git`.
- `upstream` must be `https://github.com/kaomei/stickman-video-director.git`.
- Fetch both remotes before starting work. Never push to `upstream`.
- Do not merge or push work directly to `main` unless the user explicitly requests it.
- Start each independent task on a branch named `codex/<short-task-slug>`, normally based on `upstream/main` unless continuing an existing unmerged task.
- Push task branches only to `origin` and set upstream tracking there.
- Choose concise imperative commit subjects and split commits by coherent task. Do not ask the user to choose routine branch names or commit messages.

## Skill contents

Keep the complete Skill package together when changing or reinstalling it, including:

- `SKILL.md`
- `agents/` and plugin or manifest metadata
- `scripts/`
- `references/`
- `assets/`
- `templates/` when present
- `examples/`, tests, `package.json`, and `package-lock.json`

Do not copy only `SKILL.md`; relative references and runtime files are part of the Skill contract.

## Validation before commit

1. Check that `SKILL.md` starts with valid YAML front matter containing the expected `name` and `description`.
2. Resolve every local Markdown link and referenced script/path from the file that declares it. Reject missing or path-escaping references.
3. From `skills/directing-stickman-videos/`, run `npm ci` when dependencies are absent or the lockfile changed, then run:
   - `npm test`
   - `npm run doctor`
   - a representative offline `validate` or `preview` command when renderer behavior changed
4. Invoke the installed Skill by its real name before committing, for example with a read-only, no-media smoke prompt beginning with `$directing-stickman-videos`. Confirm that Codex reports the expected A/B routing without making paid or external generation calls.
5. Review `git diff`, `git status`, and staged files. Do not commit generated outputs, secrets, unrelated changes, or files from the installed/cache copy.
6. Commit only after all applicable checks pass, then push the task branch to `origin`. Do not merge it.

## Installed-development link

- Prefer a Windows directory junction or symlink from `~/.codex/skills/directing-stickman-videos` to this checkout's `skills/directing-stickman-videos/` directory.
- Keep a dated backup of the previous installed directory until its meaningful contents have been verified or migrated.
- After relinking, rerun the doctor and real-name smoke invocation so the active Codex installation is proven to use the development copy.
