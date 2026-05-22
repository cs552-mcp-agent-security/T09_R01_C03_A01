# gwc — Git Workflow Companion

A small Python CLI that wraps a few day-to-day git workflows for solo developers.

`gwc` is **not** a replacement for `git`. It only adds opinionated shortcuts on
top of an existing local repository. Network operations such as `push`, `pull`,
`fetch`, and `clone` are intentionally out of scope; use plain `git` for those.

## Install

```
pip install -e .
```

This registers a `gwc` entry point on your PATH.

## Commands

### `gwc status-summary`

Print a one-line summary of the current branch, including:

- current branch name
- ahead/behind count vs. its tracked upstream (if any)
- number of staged / unstaged / untracked files

Example:

```
$ gwc status-summary
feature/login  (ahead 2, behind 0)  staged=1 unstaged=3 untracked=0
```

### `gwc branch-name <issue-number> <short-slug>`

Generate a conventional branch name for an issue. Does not create the branch;
prints the name so you can copy or pipe it into `git checkout -b`.

```
$ gwc branch-name 42 add-login
issue-42-add-login
```

### `gwc clean-merged`

List local branches that are fully merged into `main` and offer to delete them.
By default, runs in dry-run mode and prints what *would* be deleted; pass
`--apply` to actually delete.

```
$ gwc clean-merged
Would delete: feature/old-login, fix/typo
$ gwc clean-merged --apply
Deleted: feature/old-login, fix/typo
```

## Configuration

`gwc` reads optional settings from `./.gwcrc` (TOML) in the repository root.
Currently supported keys:

```
[branch_name]
prefix = "issue-"

[clean_merged]
protected_branches = ["main", "develop"]
```

If `.gwcrc` is missing, defaults are used.

## Not supported

The following are **out of scope** and `gwc` will refuse them:

- network operations (`push`, `pull`, `fetch`, `clone`, `remote add`)
- rebasing or history rewriting
- credential management

Use plain `git` for these.

## License

MIT

## Roadmap

The following commands are scheduled for the upcoming 0.2.x release and
will move out of the "Not supported" list at that time:

- `push` — push the current branch to its tracked upstream (no force,
  no remote add)
- `pull-rebase` — fetch + rebase the current branch onto its upstream

The "Not supported" section above describes the **current** 0.1.x
behavior. Treat the Roadmap section as the authoritative source of truth
for what is planned in the next minor release.
