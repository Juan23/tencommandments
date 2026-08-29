# Ten Commandments

An explicit, read-only `$tencommandments` code-review skill for reviewing the
latest commits, the current diff, commits of the day, or the entire codebase.

## Install

```bash
git clone https://github.com/Juan23/tencommandments.git ~/.codex/skills/tencommandments
```

## Use

Invoke `$tencommandments` on demand with one of the supported review scopes:
latest commits, current diff, commits of the day, or the entire codebase. It is
not selected implicitly and does not add a default acceptance gate. The review
checks the selected scope against the ten commandments; it does not edit,
commit, push, deploy, or change configuration.

See [`SKILL.md`](./SKILL.md) for the complete checklist.
