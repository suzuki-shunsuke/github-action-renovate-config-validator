# github-action-renovate-config-validator

GitHub Actions for renovate-config-validator

## Requirement

npx

## Input

Please see [action.yaml](action.yaml) too.

### `strict`

required: false

The input was introduced from v1.0.0.
Either `true` of `false`.
If it's `true`, renovate-config-validator's --strict option is set.
The default is `true`.

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
* .renovaterc.json5
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
      - uses: suzuki-shunsuke/github-action-renovate-config-validator@v1.0.1
```

You can specify renovate-config-validator version and configuration file path.

```yaml
steps:
  - uses: suzuki-shunsuke/github-action-renovate-config-validator@v1.0.1
    with:
      validator_version: "31.15.0"
      config_file_path: renovate.json5
      strict: "false"
```

## License

[MIT](LICENSE)
