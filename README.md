# ubc GitHub Action

[![CI](https://github.com/useblocks/ubc-action/actions/workflows/ci.yml/badge.svg)](https://github.com/useblocks/ubc-action/actions/workflows/ci.yml)
[![Check Demo](https://github.com/useblocks/ubc-action/actions/workflows/check-demo.yml/badge.svg)](https://github.com/useblocks/ubc-action/actions/workflows/check-demo.yml)

This action downloads and installs a specific version of the [`ubc` CLI tool](https://ubcode.useblocks.com/ubc/introduction.html), adds it to the `PATH`, and executes a specified `ubc` command. Checkout the [official documentation](https://ubcode.useblocks.com/ubc/usage.html) for a list of supported commands.

It supports caching the `ubc` binary to speed up workflow runs.

## Inputs

The following inputs can be used to configure the action:

| Input               | Description                                                                          | Default     | Required |
| ------------------- | ------------------------------------------------------------------------------------ | ----------- | -------- |
| `version`           | The version of the `ubc` CLI to install. Use `latest` for the latest version.        | `0.19.0`    | No       |
| `license-key`       | The ubCode license key. Recommended to be passed via secrets.                        | `''`        | No       |
| `license-user`      | The ubCode license user. Recommended to be passed via secrets.                       | `''`        | No       |
| `working-directory` | The working directory to run the `ubc` command in. Defaults to the repository root.  | `.`         | No       |

Note: For private projects setting `license-key` and `license-user` is required.

## Usage

Here is an example of how to use the `ubc-action` in your workflow to run `ubc check` on a project located in the `demo` directory.

```yaml
# filepath: .github/workflows/check-demo.yml
name: Check demo project with ubc action
on: [push, pull_request]
permissions:
  id-token: write
  contents: read
jobs:

  check:
    runs-on: ubuntu-latest
    
    steps:
      - name: Checkout
        uses: actions/checkout@v5.0.0
        with:
          submodules: "true"

      - name: Setup ubc
        uses: ./
        with:
          license-key: ${{ secrets.UBC_LICENSE_KEY }}
          license-user: ${{ secrets.UBC_LICENSE_USER }}

      - name: Test action with default settings
        working-directory: ./demo
        run: ubc check .
```

## License

The scripts and documentation in this project are released under the [MIT License](LICENSE).