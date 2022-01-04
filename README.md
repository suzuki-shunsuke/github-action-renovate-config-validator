# github-action-renovate-config-validator

GitHub Actions for renovate-config-validator

## Input and Output

Please see [action.yaml](action.yaml)

## Example

```yaml
steps:
  - uses: suzuki-shunsuke/github-action-renovate-config-validator
```

You can specify renovate-config-validator version and configuration file path.

```yaml
steps:
  - uses: suzuki-shunsuke/github-action-renovate-config-validator
    with:
      validator_version: "31.15.0"
      config_file_path: renovate.json5
```

## Liencse

[MIT](LICENSE)
