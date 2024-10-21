# Build & Push Helm chart - GitHub Action

This GitHub Action automates the process of building a Helm chart package, logging into a Helm registry, and pushing the chart to the registry.

## Inputs

| Name            | Required | Description                                                 |
|-----------------|----------|-------------------------------------------------------------|
| `registry_url`  | `true`   | The URL of the Helm registry (e.g., `registry1.azurecr.io`) |
| `client_id`     | `true`   | The Client ID used for logging into the Helm registry       |
| `client_secret` | `true`   | The Client Secret used for logging into the Helm registry   |
| `chart_path`    | `true`   | The path to the Helm chart directory                        |

## Usage

Here’s an example of how you can use this action in your workflow:

```yaml
name: Build and Push Helm Chart

on:
  push:
    branches:
      - main

jobs:
  build-and-push:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout code
        uses: actions/checkout@v4
        with:
          fetch-depth: 0

      - name: Build & Push Helm Chart to Registry
        uses: rewe-digital-ri/hol-github/actions/build-push-helm-chart@v1
        with:
          registry_url: registry1.azurecr.io
          client_id: ${{ secrets.REGISTRY_CLIENT_ID }}
          client_secret: ${{ secrets.REGISTRY_CLIENT_SECRET }}
          chart_path: './path/to/your/chart'

