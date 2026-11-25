# Renovate Config

This repository groups presets containing common and specific [Renovate Bot](https://docs.renovatebot.com/config-presets/#github-hosted-presets) configuration for our projects.

## Usage

In the project's `renovate.json` file, extends any of these presets and add your own additional configuration if needed.

### Available presets

<details>
  <summary>Default</summary><br>

<p>Uses the <a href="https://docs.renovatebot.com/presets-config/#configbase">config:base</a> preset and enables the following:</p>

<ul>
<li>PRs are scheduled to be created every Monday before 3am (UTC), four at the time.</li>
<li>Stale PRs are automatically rebased.</li>
<li><code>chore({depType})</code> (e.g. <code>chore(dev-deps)</code>) is used for PR titles and commit messages.</li>
</ul>

```json
{
  "extends": ["github>gr4vy/renovate-config"]
}
```

</details>

<details>
  <summary>JavaScript</summary><br>

<p>Uses the default preset and enables the following:</p>

<ul>
<li>Dependencies ranges are bumped (e.g. <code>^1.0.0 -> ^1.1.0</code>).</li>
<li>PRs are automatically assigned to <a href="https://github.com/orgs/gr4vy/teams/javascript-team">the JavaScript team</a> as reviewer.</li>
<li>Special security PRs are created for vulnerabilities in root packages.</li>
<li>Development dependencies are labelled with `patch` and automerged.</li>
<li>Production dependencies are labelled with `minor`.</li>
<li>Resolutions and major dependency updates are disabled.</li>
<li><code>eslint</code> updates are grouped.</li>
</ul>

```json
{
  "extends": ["github>gr4vy/renovate-config:javascript"]
}
```

</details>

<details>
  <summary>Base (<code>presets/base.jsonc</code> + <code>presets/base/*.jsonc</code>)</summary><br>

<p>A modular set of presets meant to be used together: always extend <code>presets/base.jsonc</code>, then add one preset from <code>presets/base/</code> for each ecosystem the repository uses. They are independent of the default preset above.</p>

<p><code>presets/base.jsonc</code> uses <a href="https://docs.renovatebot.com/presets-config/#configrecommended">config:recommended</a> and enables the following:</p>

<ul>
<li>PRs are opened Monday to Thursday, 9:00–19:59 (Europe/London), at most 10 open at a time.</li>
<li>Updates wait 7 days after release; vulnerability fixes skip every schedule and are labelled <code>vulnerability</code>.</li>
<li><code>chore(deps): ...</code> is used for PR titles and commit messages.</li>
<li>Stale PRs are rebased, GitHub Actions are pinned to commit SHAs, and unmaintained dependencies are flagged.</li>
<li>Ranges are bumped (e.g. <code>^1.0.0 -> ^1.1.0</code>).</li>
<li>PRs are labelled <code>dependencies</code> plus <code>major</code>, <code>minor</code> or <code>patch</code>.</li>
<li>Commits are created through the GitHub API, so they are signed (verified).</li>
<li>When a rule enables automerge, Renovate turns on GitHub's auto-merge for the PR (squash) and assigns no one; GitHub merges it once its required checks pass. Renovate only creates, rebases and updates branches during the schedule, so a PR that falls behind <code>main</code> waits for the next window to be brought up to date. In practice, auto-merged PRs land during schedule hours too, except in edge cases such as checks that finish just after the window closes.</li>
</ul>

<p>Add-ons in <code>presets/base/</code>:</p>

<ul>
<li><code>docker.jsonc</code>: Docker Compose and Dockerfile updates need Dependency Dashboard approval.</li>
<li><code>github-actions.jsonc</code>: updates wait 14 days after release; patch updates are automerged.</li>
<li><code>python.jsonc</code>: Python language, dev (PEP 735 <code>[dependency-groups].dev</code> and Poetry <code>[tool.poetry.group.dev.dependencies]</code>) and OpenTelemetry updates are grouped; OpenTelemetry packages already on a prerelease (e.g. <code>opentelemetry-instrumentation-*</code> betas) can update to newer prereleases; patch updates, minor updates of 1.x+ packages and dev minor/patch updates are automerged; major updates are not; Python language updates need Dependency Dashboard approval.</li>
</ul>

<p>Preset files use the <code>.jsonc</code> extension, so it must be included in the path. Keep <code>presets/base.jsonc</code> first, and add <code>reviewers</code> and any repository-specific <code>packageRules</code> (these are applied after the presets', so they can override them):</p>

```jsonc
{
  "$schema": "https://docs.renovatebot.com/renovate-schema.json",
  "extends": [
    "github>gr4vy/renovate-config//presets/base.jsonc",
    "github>gr4vy/renovate-config//presets/base/docker.jsonc",
    "github>gr4vy/renovate-config//presets/base/github-actions.jsonc",
    "github>gr4vy/renovate-config//presets/base/python.jsonc"
  ],

  "reviewers": ["team:my-team"],

  "packageRules": [
    // Automerge
    {
      "description": "Disable automerge for specific Python major/minor updates",
      "matchCategories": ["python"],
      "matchUpdateTypes": ["major", "minor"],
      "matchPackageNames": ["sentry-sdk"],
      "automerge": false
    }
  ]
}
```

</details>
