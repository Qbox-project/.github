# Qbox-project/.github

Central configuration for the [Qbox-project](https://github.com/Qbox-project) GitHub organization. This repository replaces the former `github-docs` file-sync approach with native GitHub org defaults and reusable workflows.

## Repository layout

```
Qbox-project/.github/           (this repo)
├── profile/README.md           Org profile page
├── README.md                   This guide
└── .github/
    ├── CODE_OF_CONDUCT.md      Org-wide default
    ├── contributing.md         Org-wide default
    ├── pull_request_template.md Org-wide default
    ├── ISSUE_TEMPLATE/         Org-wide default
    └── workflows/              Reusable workflow_call workflows
```

All org defaults and shared workflows live under `.github/` in this repo. GitHub serves these as organization-wide defaults for any repo that does not define its own copies. See [Creating a default community health file](https://docs.github.com/en/communities/setting-up-your-project-for-healthy-contributions/creating-a-default-community-health-file).

## Reusable workflows

Resource repos call these via `workflow_call`:

```yaml
jobs:
  lint:
    uses: Qbox-project/.github/.github/workflows/lint.yml@main
    secrets: inherit
```

| Workflow | Inputs | Secrets |
|----------|--------|---------|
| `lint.yml` | — | — |
| `release.yml` | `version` (required) | `APP_ID`, `PRIVATE_KEY` |
| `release-action.yml` | `tag` (optional) | `APP_ID`, `PRIVATE_KEY` |
| `discord-commit.yml` | — | `WEBHOOK_ID`, `WEBHOOK_TOKEN` |
| `discord-release.yml` | — | `WEBHOOK_URL` |
| `issues-project.yml` | — | `APP_ID`, `PRIVATE_KEY` |
