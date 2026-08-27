# Ten Commandments

An explicit, read-only `$tencommandments` code-review skill derived from the
Practical Power of Ten correctness bar formerly bundled with JM.

## Install

```bash
git clone https://github.com/Juan23/tencommandments.git ~/.codex/skills/tencommandments
```

## Use

Invoke `$tencommandments` when a focused correctness review is wanted. It is
not selected implicitly and does not add a default acceptance gate. The review
checks the changed behavior against the ten bounded correctness commandments;
it does not edit, commit, push, or deploy.

See [`SKILL.md`](./SKILL.md) for the complete checklist.
