# Renovate Config

This repository groups presets containing common and specific [Renovate Bot](https://docs.renovatebot.com/config-presets/#github-hosted-presets) configuration for our projects.

## Usage

In the project's `renovate.json` file, extends any of these presets and add your own additional configuration if needed.

### Available presets

<details>
  <summary>Default</summary><br>

```json
{
  "extends": ["github>gr4vy/renovate-config"]
}
```

</details>

<details>
  <summary>JavaScript</summary><br>

```json
{
  "extends": ["github>gr4vy/renovate-config:javascript"]
}
```

</details>
