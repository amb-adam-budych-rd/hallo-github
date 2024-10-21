# Build & Push Docker Image - GitHub Action

This GitHub Action automates the process of building a Docker image for your application, logging into a Docker registry, and pushing the image to the registry with versioning based on the current Git commit.

## Inputs

| Name               | Required | Description                                                   |
|--------------------|----------|---------------------------------------------------------------|
| `registry_url`     | `true`   | The URL of the Docker registry (e.g., `registry1.azurecr.io`) |
| `client_id`        | `true`   | The Client ID used for logging into the Docker registry       |
| `client_secret`    | `true`   | The Client Secret used for logging into the Docker registry   |
| `dockerfile_path`  | `true`   | The path to the Dockerfile                                    |
| `application_name` | `true`   | The name of the application being containerized               |

## Steps Overview

1. **Calculate Version**: Generates a short Git commit SHA to use as a unique tag for the Docker image.
2. **Login to Docker Registry**: Logs into your Docker registry using the provided credentials.
3. **Build and Push Docker Image**: Builds the Docker image using the provided Dockerfile and pushes it to the registry. The image is tagged with both the short SHA and the `latest` tag.

## Usage

Here’s an example of how you can use this action in your GitHub workflow:

```yaml
name: Build and Push Docker Image

on:
  push:
    branches:
      - main

jobs:
  build-and-push:
    runs-on: ubuntu-latest

    steps:
      - uses: actions/download-artifact@master
        with:
          name: jar-file
          path: app.jar

      - name: Build & Push Docker Image to Registry
        uses: rewe-digital-ri/hol-github/actions/build-push-docker-image@v1
        with:
          registry_url: registry1.azurecr.io
          client_id: ${{ secrets.REGISTRY_CLIENT_ID }}
          client_secret: ${{ secrets.REGISTRY_CLIENT_SECRET }}
          dockerfile_path: './Dockerfile'
          application_name: 'my-application'
