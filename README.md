# Renovate Config

This repository groups presets containing common and specific [Renovate Bot](https://docs.renovatebot.com/config-presets/#github-hosted-presets) configuration for our projects.

## Usage

In the project's `renovate.json` file, extends any of these presets and add your own additional configuration if needed.

### Available presets

<details>
  <summary>Default</summary><br>

<p>Uses the <a href="https://docs.renovatebot.com/presets-config/#configbase">config:base</a> preset and enables the following:</p>

<ul>
<li>PRs are scheduled to be created every Monday before 3am (UTC), four at the time</li>
<li>Stale PRs are automatically rebased</li>
<li>`chore({depType})` (e.g. `chore(dev-deps)`) is used for PR titles and commit messages</li>
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
<li>Dependencies ranges are bumped (e.g. `^1.0.0 -> ^1.1.0`)</li>
<li>PRs are automatically assigned to <a href="https://github.com/orgs/gr4vy/teams/javascript-team">the JavaScript team</a> as reviewer</li>
<li>Special security PRs are created for vulnerabilities in root packages</li>
<li>Development dependencies are labelled with `patch` and automerged</li>
<li>Production dependencies are labelled with `minor`</li>
<li>Resolutions and major dependency updates are disabled</li>
<li>`eslint` updates are grouped</li>
</ul>

```json
{
  "extends": ["github>gr4vy/renovate-config:javascript"]
}
```

</details>
