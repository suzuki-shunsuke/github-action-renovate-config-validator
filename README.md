# github-action-renovate-config-validator

GitHub Actions for renovate-config-validator

## Requirement

npx

## Input

Please see [action.yaml](action.yaml) too.

### `validator_version`

required: false

The version of renovate-config-validator.
By default, the latest version is used.

### `config_file_path`

required: false

Renovate Configuration file path.
By default, the following files are validated.

* .github/renovate.json
* .github/renovate.json5
* .gitlab/renovate.json
* .gitlab/renovate.json5
* .renovaterc.json
* renovate.json
* renovate.json5
* .renovaterc

## Output

Nothing.

## Example

```yaml
name: renovate-config-validator

on:
  pull_request:
    branches:
      - main
  push:
    branches:
      - main
jobs:
  validate:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2
      - uses: suzuki-shunsuke/github-action-renovate-config-validator@v0.1.2
```

You can specify renovate-config-validator version and configuration file path.

```yaml
steps:
  - uses: suzuki-shunsuke/github-action-renovate-config-validator@v0.1.2
    with:
      validator_version: "31.15.0"
      config_file_path: renovate.json5
```

## Liencse

[MIT](LICENSE)
