# github-action-renovate-config-validator

GitHub Actions and reusable workflow for renovate-config-validator

[action.yaml](action.yaml), [Reusable workflow](.github/workflows/renovate-config-validator.yaml)

This action runs [@suzuki-shunsuke/renovate-config-validator](https://github.com/suzuki-shunsuke/renovate-config-validator), renovate-config-validator bundled into a single file.
It's installed in a few seconds, while installing renovate takes tens of seconds.
The version of @suzuki-shunsuke/renovate-config-validator is the same as the version of renovate.

## :rocket: Recent Update

- v2.4.0: Support the reusable workflow
- v2.3.0: :zap: Much faster. Validation finishes in a few seconds instead of tens of seconds (19s → 6s in our CI) [#1224](https://github.com/suzuki-shunsuke/github-action-renovate-config-validator/pull/1224)
- v2.2.0: JSONC configuration files such as renovate.jsonc are validated by default
- v2.1.0: Cache ~/.npm by default, which improves the performance and mitigates API rate limit issues
- v2.0.0: Node.js 24 is installed by default to support the latest Renovate

## Input

Please see [action.yaml](action.yaml) too.

## `node-version`

required: false

The input was introduced from v2.0.0.
As of v2.0.0, this action installs Node.js using actions/setup-node by default.
If `node-version` is `none`, the installation is skipped.
We started installing Node.js by default because the Node.js version which is pre-installed into GitHub Actions `ubuntu-latest` runner is old (v20) and doesn't support the latest Renovate.

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
If @suzuki-shunsuke/renovate-config-validator of the version isn't published, renovate is installed instead.

### `config_file_path`

required: false

Renovate Configuration file path.
By default, the following files are validated.

* .github/renovate.json
* .github/renovate.jsonc
* .github/renovate.json5
* .gitlab/renovate.json
* .gitlab/renovate.jsonc
* .gitlab/renovate.json5
* .renovaterc.json
* .renovaterc.jsonc
* .renovaterc.json5
* renovate.json
* renovate.jsonc
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

### `npm_cache`

required: false

Enable npm cache.
If it's "true", the npm cache is enabled.
By default, the npm cache is disabled.

The npm cache was enabled by default to speed up the installation of renovate and reduce 403 errors from the npm registry ([#1091](https://github.com/suzuki-shunsuke/github-action-renovate-config-validator/issues/1091)).
But the bundled renovate-config-validator is small enough and depends on few packages, so the npm cache is usually unnecessary.
It may be useful when renovate is installed because @suzuki-shunsuke/renovate-config-validator of `validator_version` isn't published.

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
