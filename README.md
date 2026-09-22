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
| `release-artifacts.yml` | `artifact-pattern` (required), `tag` (optional) | `APP_ID`, `PRIVATE_KEY` |
| `discord-commit.yml` | — | `DISCORD_COMMIT_WEBHOOK` |
| `discord-release.yml` | — | `WEBHOOK_URL` |
| `issues-project.yml` | — | `APP_ID`, `PRIVATE_KEY` |

### Compiled tools and editor packages

Use `release-artifacts.yml` for repositories that build binaries or VSIX packages. Keep the
platform build jobs in the calling repository and upload their archives with
`actions/upload-artifact@v4`. Once every build succeeds, call the shared publisher:

```yaml
publish:
  needs: build
  uses: Qbox-project/.github/.github/workflows/release-artifacts.yml@main
  with:
    artifact-pattern: binary-*
  secrets: inherit
```

The caller must run on the release tag and allow `contents: read`. The publisher accepts flat
ZIP, tar.gz and VSIX files, adds `SHA256SUMS`, and verifies that the tag matches the build's
source commit. Release notes link to the changelog at that tag. The GitHub App must be installed
on the calling repository with permission to write repository contents.

This uses the App token so publishing a release also triggers a caller's `release: published`
workflow. Call `discord-release.yml` from that event to announce releases; call
`discord-commit.yml` on pushes to `main` for commit notifications. Both callers use
`secrets: inherit`.
