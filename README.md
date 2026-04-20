# WireMock Cloud Data Source Automation

This project demonstrates how to use GitHub Actions to automatically update a [WireMock Cloud](https://wiremock.io) data source when a CSV file is edited in the repository.

## How It Works

When a CSV file in the `data/` directory is pushed to the `main` branch, a GitHub Actions workflow fires and:

1. **Detects** which CSV files were added or modified in the push
2. **Finds** the existing WireMock Cloud data source whose name matches the CSV filename (without extension)
3. **Parses** the updated CSV using the WireMock Cloud parse API
4. **Updates** the data source via the WireMock Cloud REST API

The workflow enforces that exactly one data source matches the filename. It will fail explicitly if no match is found or if multiple matches are found, preventing accidental updates to the wrong data source.

## Prerequisites

- A [WireMock Cloud](https://wiremock.io) account
- A WireMock Cloud API token

## Repository Secrets

Configure the following secret in your GitHub repository (**Settings → Secrets and variables → Actions**):

| Secret | Description |
|---|---|
| `WIREMOCK_CLOUD_API_TOKEN` | Your WireMock Cloud API token |

## Repository Structure

```
data/
└── Products.csv        # CSV files here trigger the workflow on push
.github/
└── workflows/
    └── add_update_data_source.yml   # GitHub Actions workflow
```

Any CSV file placed in the `data/` directory will be watched. The filename (without `.csv`) must match the name of an existing data source in your WireMock Cloud account.

## Usage

1. Fork or clone this repository
2. Add the required secret to your GitHub repository
3. Edit or add a CSV file under `data/` and push to `main`
4. The GitHub Actions workflow will detect the change and update the matching WireMock Cloud data source automatically or create a new data source

## Workflow Trigger

The workflow runs on any push to `main` that includes changes under the `data/` path:

```yaml
on:
  push:
    branches:
      - main
    paths:
      - 'data/**'
```

