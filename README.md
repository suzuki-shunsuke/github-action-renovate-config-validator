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

If you want to validate multiple files, you can pass multile lines.
Leading spaces on each line are removed.

```yaml
with:
  config_file_path: |
    default.json
    foo.json
```

You can pass `config_file_path` through output command.

```yaml
      - id: files
        run: |
          set -euo pipefail
          files=$(git ls-files | grep renovate.json)
          # https://stackoverflow.com/a/74232400
          EOF=$(dd if=/dev/urandom bs=15 count=1 status=none | base64)
          {
            echo "files<<$EOF"
            echo "$files"
            echo "$EOF"
          } >> "$GITHUB_OUTPUT"
      - name: Pass files through output
        uses: suzuki-shunsuke/github-action-renovate-config-validator@v1.1.0
        with:
          config_file_path: ${{ steps.files.outputs.files }}
```

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
