# Renovate configuration preset repository

[![GitHub release](https://img.shields.io/github/release/3ware/renovate-config?include_prereleases=&sort=semver&color=green)](https://github.com/3ware/renovate-config/releases/) [![issues - workflows](https://img.shields.io/github/issues/3ware/renovate-config)](https://github.com/3ware/renovate-config/issues) [![CI](https://img.shields.io/github/actions/workflow/status/3ware/renovate-config/pr-check.yaml?label=CI&logo=githubactions&logoColor=white)](https://github.com/3ware/renovate-config/actions/workflows/pr-check.yaml)

## Overview

This repository contains reusable Renovate configuration presets that can be shared across multiple projects to standardize dependency update policies.

## Example Usage

To use a preset from this repository in your project, add it to your `.github/renovate.json` file:

```json
{
  "$schema": "https://docs.renovatebot.com/renovate-schema.json",
  "extends": ["local>3ware/renovate-config"]
}
```

## Preset Configuration

The default preset applies the following policies:

| Element                        | Description                                                                                                                                                                                          |
| ------------------------------ | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `extends`                      | Enables Renovate's `config:best-practices` preset.                                                                                                                                                   |
| `addLabels`                    | Adds the dependency manager name, such as `github-actions`, to pull requests.                                                                                                                        |
| `assigneesFromCodeOwners`      | Assigns pull requests to the people responsible for the changed files in `CODEOWNERS`.                                                                                                               |
| `dependencyDashboardAutoclose` | Automatically closes the Dependency Dashboard when no outstanding updates remain.                                                                                                                    |
| `minimumReleaseAge`            | Waits 14 days after a release before proposing an update; reduces risk of updating to a vulnerable package.                                                                                          |
| `terraform.enabled`            | Disables Renovate's Terraform manager by default. Custom manager updates tofu version in trunk.yaml, setup-ci workflow and terrateam config.                                                         |
| `customManagers`               | Uses a regex manager to update the `vX.Y.Z` version in `.github/gh-actions-lock.version` from GitHub releases for `github/gh-actions-lock`. Versions use Renovate's default `semver-coerced` scheme. |
| `schedule`                     | Runs scheduled updates from 19:00 to 08:00 across Sunday night and Monday morning.                                                                                                                   |
| `vulnerabilityAlerts`          | Processes vulnerability updates at any time, labels them `security`, and uses the standard build commit prefix.                                                                                      |

### Package Rules

- GitHub Actions versions are considered abandoned after two years without a release.
- Minor, patch, pin and digest updates are grouped by manager, labelled `auto-merge`, and kept separate from major updates. They are not currently auto-merged.
- Major updates are labelled `review-required`, are not auto-merged, and use a breaking-change commit prefix containing the dependency name.
- Action-lock updates use the regex custom manager, so they follow these generic update rules; their commit prefix is `build(actionlock):`.
- GitHub Actions updates use the `ci` commit prefix; TFLint plugin updates use `chore`.
- The OpenTofu `uses-with` dependency is disabled for GitHub Actions. This prevents updates to the setup-ci action independently of the tofu custom manager
- Updates to `ghcr.io/terrateamio/action` are checked immediately and run on demand.
