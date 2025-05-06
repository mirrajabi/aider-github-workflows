# Aider Issue to PR Workflow

Don't waste time writing code for simple features anymore! Just tag your issues and let AI Automatically generate a PR for you!

See [a demo](#example) of how it works.

## Usage

The recommended way to use this workflow is to call it when an issue is labeled.

1. Set `OPENAI_API_KEY` secret in your repository settings. You can get your API key from [OpenAI](https://platform.openai.com/api-keys).
1. Create file `.github/workflows/aider-on-issue-labeled.yml` in your project with the following contents:

```yaml
name: Auto-generate PR using Aider
on:
  issues:
    types: [labeled]

# grants the workflow read access to issues, write access to pull requests, and write access to the repository's contents
permissions:
  issues: read
  pull-requests: write
  contents: write

jobs:
  generate:
    uses: mirrajabi/aider-github-workflows/.github/workflows/aider-issue-to-pr.yml@v1.0.0
    # Check if the label is 'aider'
    if: github.event.label.name == 'aider'
    with:
      issue-number: ${{ github.event.issue.number }}
      base-branch: ${{ github.event.repository.default_branch }}
      # Exit if the action is taking longer than 10 minutes
      chat-timeout: 10
      api_key_env_name: OPENAI_API_KEY
      model: o3-mini-2025-01-31
    secrets: 
      api_key_env_value: ${{ secrets.OPENAI_API_KEY }}

```

### Setup OPENAI_API_KEY

`OPENAI_API_KEY` should be set in Secrets in the GitHub repository.

1. Enter your GitHub repository.
2. Click the "Settings" tab.
3. In the left navigation bar, find the "Security" section, click "Secrets and variables" and select "Actions".
4. Click the "New repository secret" button.
5. Enter `OPENAI_API_KEY` in the "Name" field.
6. Enter your OpenAI API key in the "Secret" field.
7. Click the "Add secret" button.

This way, your GitHub Action can safely access your OpenAI API key. Your workflow file (the yaml file you provide) has been configured to use this secret. It will be read from the environment variable `secrets.OPENAI_API_KEY`.

## Inputs

You can pass the following arguments to the workflow:

| Field Name     | Description                                                   | Required | Type    | Default                  |
|-----------------|---------------------------------------------------------------|----------|---------|--------------------------|
| `base-branch`     | Base branch to create PR against                               | **true**     | string  | -                        |
| `chat-timeout`    | Timeout for chat in *minutes* to prevent burning your credits    | false    | number  | `10`                       |
| `issue-number`    | Issue number                                                  | **true**     | number  | -                        |
| `api_key_env_name`   | "The name of the environment variable. For example, OPENAI_API_KEY, ANTHROPIC_API_KEY, etc. See more info [here](https://aider.chat/docs/llms.html)       | **false** | string  | -                        |
| `model`           | Model to use                                                  | true    | string  | -    |

### Secrets

| Field Name     | Description                                                   | Required | Type    | Default                  |
|-----------------|---------------------------------------------------------------|----------|---------|--------------------------|
| `api_key_env_value` | The API Key to use as the value of the `api_key_env_name`   | **false** | string  | -  |

## Example

In [aider-github-workflows-test](https://github.com/mirrajabi/aider-github-workflows-test/issues) project you can find the following which demonstrate the full flow from Issue to generated PR.

1. [An issue](https://github.com/mirrajabi/aider-github-workflows-test/issues/1) that triggers the workflow.
1. [The workflow run](https://github.com/mirrajabi/aider-github-workflows-test/actions/runs/7365235652)
1. [A Pull request](https://github.com/mirrajabi/aider-github-workflows-test/pull/2) that was created

## Roadmap

- [ ] Add support for in PR chat and revisions

## Disclaimer

You are responsible for the AI credit that is spent. All contributors in a GitHub project will be able to label issues which will trigger the workflow. Non-contributors are not be able to tag issues which helps greatly in preventing abuse.
