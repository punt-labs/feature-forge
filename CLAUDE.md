# Agent Instructions

This is a prompt-only Claude Code plugin — no application code, no runtime. The feature development workflow is defined in skill prompts and agent configurations.

This project follows [Punt Labs standards](https://github.com/punt-labs/punt-kit).

## Quality Gates

```bash
npx markdownlint-cli2 "**/*.md" "#node_modules"
```

All markdown must pass markdownlint before commit. CI enforces this via `docs.yml`.

## Standards References

- [GitHub](https://github.com/punt-labs/punt-kit/blob/main/standards/github.md)
- [Workflow](https://github.com/punt-labs/punt-kit/blob/main/standards/workflow.md)
- [Plugins](https://github.com/punt-labs/punt-kit/blob/main/standards/plugins.md)
