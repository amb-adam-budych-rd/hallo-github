# Check is Helm chart version changed - GitHub action

This GitHub Action checks if the version in a Helm chart (`Chart.yaml`) has changed between commits. 
It compares the current commit with the previous one and sets an output based on whether the version has changed.

## Inputs

| Name         | Required | Description                                    |
|--------------|----------|------------------------------------------------|
| `chart_path` | `true`   | The path to the Helm chart's `Chart.yaml` file |

## Outputs

| Name                    | Description                                                               |
|-------------------------|---------------------------------------------------------------------------|
| `chart_version_changed` | A boolean (`true` or `false`) indicating if the chart version has changed |

## Usage

Here’s how you can use this action in your workflow:

```yaml
name: Name of your workflow

on:
    push:
        branches:
            - main

jobs:
  check-version:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout code
        uses: actions/checkout@v4
        with:
            fetch-depth: 0

      - name: Check if Helm chart version has changed
        id: check-version
        uses: rewe-digital-ri/hol-github/actions/check-helm-chart-version-changed@v1
        with:
          chart_path: './path/to/your/project/Chart.yaml'

      - name: Run step on condition
        if: steps.check-version.outputs.chart_version_changed == 'true'
        run: echo "Condition step was executed"
