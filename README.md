<p align="center"><img src="images/kubesec_logo.svg" width="200x" /></p>

# Kubesec Action

> [GitHub Action](https://github.com/features/actions) for [kubesec](https://github.com/controlplaneio/kubesec)

[![GitHub Release][release_badge]][release]
[![GitHub Marketplace][marketplace_badge]][marketplace]



## Table of Contents

- [Usage](#usage)
  - [Workflow](#workflow)
- [Customizing](#customizing)
  - [Inputs](#inputs)

## Usage

### Workflow

```yaml
name: lint
on:
  push:
    branches:
      - master
  pull_request:
jobs:
  lint:
    name: Lint
    runs-on: ubuntu-latest
    steps:
      - name: Checkout code
        uses: actions/checkout@v2

      - name: Run kubesec scanner
        uses: controlplaneio/kubesec-action@v0.0.2
        with:
          input: file.yaml
```

### Using kubesec with GitHub Code Scanning

If you have [GitHub code scanning][code_scanning] available you can use kubesec as a scanning tool as follows:

```yaml
name: lint
on:
  push:
    branches:
      - master
  pull_request:
jobs:
  lint:
    name: Lint
    runs-on: ubuntu-latest
    steps:
      - name: Checkout code
        uses: actions/checkout@v2

      - name: Run kubesec scanner
        uses: controlplaneio/kubesec-action@v0.0.2
        with:
          input: file.yaml
          exit-code: "0"
          format: template
          template: template/sarif.tpl
          output: kubesec-results.sarif

      - name: Upload Kubesec scan results to GitHub Security tab
        uses: github/codeql-action/upload-sarif@v1
        with:
          sarif_file: kubesec-results.sarif
```

## Customizing

### Inputs

Following inputs can be used as `step.with` keys:

| Name        | Type   | Default | Description                              |
| ----------- | ------ | ------- | ---------------------------------------- |
| `input`     | String |         | File to scan                             |
| `format`    | String | `json`  | Output format (`json`, `template`)       |
| `template`  | String |         | Output template (`/templates/sarif.tpl`) |
| `output`    | String |         | Save results to a file                   |
| `exit-code` | String | `"2"`   | Override the exit-code                   |

[release]: https://github.com/controlplaneio/kubesec-action/releases/latest
[release_badge]: https://img.shields.io/github/release/controlplaneio/kubesec-action.svg?logo=github
[marketplace]: https://github.com/marketplace/actions
[marketplace_badge]: https://img.shields.io/badge/marketplace-kubesec--action-blue?logo=github
[license]: https://github.com/controlplaneio/kubesec-action/blob/master/LICENSE
[code_scanning]: https://docs.github.com/en/github/finding-security-vulnerabilities-and-errors-in-your-code/about-code-scanning
