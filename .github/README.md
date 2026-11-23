# Renovate Configuration Preset Repository

[![GitHub release](https://img.shields.io/github/release/3ware/renovate-config?include_prereleases=&sort=semver&color=green)](https://github.com/3ware/renovate-config/releases/) [![issues - workflows](https://img.shields.io/github/issues/3ware/renovate-config)](https://github.com/3ware/renovate-config/issues) [![CI](https://img.shields.io/github/actions/workflow/status/3ware/renovate-config/pr-check.yaml?label=CI&logo=githubactions&logoColor=white)](https://github.com/3ware/renovate-config/actions/workflows/pr-check.yaml)

Renovate configuration presets that can be shared across multiple projects to standardize dependency update policies.

## Configuration

The default preset applies the following policies:

| Element                         | Description                                                                                                                                                                                          |
| ------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `extends`                       | Enables Renovate's `config:best-practices` preset.                                                                                                                                                   |
| `dependencyDashboard.autoclose` | Automatically closes the Dependency Dashboard when no outstanding updates remain.                                                                                                                    |
| `ignorePresets`                 | Disables `helpers:pinGitHubActionDigests`; workflows keep version tags and `gh actions-lock` pins SHAs in `actions.lock`. Digest pinning for other managers, such as Docker, still applies.          |
| `labels`                        | Labels pull requests with the dependency manager name, such as `github-actions`.                                                                                                                     |
| `minimumReleaseAge`             | Waits 14 days after a release before proposing an update; reduces risk of updating to a compromised or broken release.                                                                               |
| `reviewersFromCodeOwners`       | Requests reviews from the code owners of the changed files in `CODEOWNERS`.                                                                                                                          |
| `terraform.enabled`             | Disables Renovate's Terraform manager. Terraform / OpenTofu updates are made at the discretion of the local repository administrator.                                                                |
| `customManagers`                | Uses a regex manager to update the `vX.Y.Z` version in `.github/gh-actions-lock.version` from GitHub releases for `github/gh-actions-lock`. Versions use Renovate's default `semver-coerced` scheme. |
| `schedule`                      | Creates update branches between 19:00 Sunday and 08:59 Monday (UTC).                                                                                                                                 |
| `vulnerabilityAlerts`           | Labels vulnerability fixes `security`. Renovate raises these outside the schedule and without the minimum release age.                                                                               |

### Package Rules

- GitHub Actions are flagged as abandoned after two years without a release.
- Minor, patch, pin, pinDigest and digest updates are grouped by manager, labelled `auto-merge`, and use the `build(<manager>): ` commit prefix.
- Major updates are labelled `review-required` and use the `build(<manager>)!: ` commit prefix with the dependency name as the topic.
- GitHub Actions updates, including majors, use the `ci(actions): ` commit prefix.
- `github/gh-actions-lock` updates are labelled `github-actions` instead of the regex manager name and use the `ci(actions): ` commit prefix.
- `tflint-plugin` updates use the `chore(<manager>): ` commit prefix.
- `ghcr.io/terrateamio/action` updates have no minimum release age and are raised at any time.

## Requirements

- The labels `auto-merge`, `review-required` and `security` must exist in the repository.
- PRs labelled `auto-merge` are approved by the auto-approve job in the PR check workflow; `review-required` PRs need manual review.

## Usage

To use a preset from this repository in your project, add it to your `.github/renovate.json` file:

```json
{
  "$schema": "https://docs.renovatebot.com/renovate-schema.json",
  "extends": ["local>3ware/renovate-config"]
}
```
