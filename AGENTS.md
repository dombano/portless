# Agent Rules

## Package Manager

Use `pnpm` for all package management commands (not npm or yarn).

Exception: End-user install instructions should use `npm install -g` (global) or `npm install -D` (project dev dependency) since npm is universal.

## Dependencies

Always check for the latest npm version when adding dependencies. Use `pnpm add <package>` (without version) to get the latest, or verify with `npm view <package> version` first.

## No Emojis

Do not use emojis anywhere in this repository (code, comments, output, docs).

## Dashes

Never use `--` as a dash in prose, comments, or user-facing output. Use an em dash (\u2014) when a dash is needed, but prefer rephrasing to avoid dashes entirely. The only exception is CLI flags (e.g. `--port`).

## Boolean Environment Variables

Document boolean env vars using only `0` and `1` in CLI help, SKILL.md, docs pages, and README. Code accepts `true`/`false` as well (and `skip` for `PORTLESS`), but these alternatives are not documented.

## Docs Updates

When a change affects how humans or agents use portless (new/changed/removed commands, flags, behavior, or config), update all of these:

1. `README.md` (user-facing documentation)
2. `skills/portless/SKILL.md` (agent skill for using portless)
3. `packages/portless/src/cli.ts` (`--help` output)

## Releasing

Releases are manual, single-PR affairs. The maintainer controls the changelog voice and format.

To prepare a release:

1. Create a branch (e.g. `prepare-v1.2.0`)
2. Bump the version in `packages/portless/package.json`
3. Write the changelog entry in `CHANGELOG.md`, wrapped in `<!-- release:start -->` and `<!-- release:end -->` markers
4. Remove the `<!-- release:start -->` and `<!-- release:end -->` markers from the previous release entry (only the latest release should have markers)
5. Add a matching entry to `apps/docs/src/app/changelog/page.mdx`
6. Open a PR and merge to `main`

CI compares the version in `packages/portless/package.json` to what's on npm. If it differs, it builds, publishes, and creates the GitHub release automatically. The release body is extracted from the content between the markers.

## Windows Debugging

A remote Windows Server 2022 EC2 instance is available for debugging Windows-specific issues. It uses AWS Systems Manager (SSM) with no SSH or open ports. Commands run via `aws ssm send-command` and return stdout/stderr.

All scripts require `AWS_PROFILE=portless-debug` (or the profile must be set as default). Prefix every command with it or export it for the session:

```bash
export AWS_PROFILE=portless-debug
```

### Prerequisites

The instance must be provisioned first (one-time, by a human):

```bash
./scripts/windows-debug/provision.sh
```

Requires: AWS CLI v2 configured with `ec2:*`, `iam:CreateRole`, `iam:AttachRolePolicy`, `ssm:SendCommand`, `ssm:GetCommandInvocation` permissions and a default VPC.

### Usage

Start the instance (if stopped):

```bash
./scripts/windows-debug/start.sh
```

Run a command on W

## Personal Notes

<!-- Personal fork: I primarily use this on macOS with Node 20. Haven't tested the Windows debugging workflow yet. -->
