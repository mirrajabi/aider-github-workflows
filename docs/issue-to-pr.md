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

jobs:
  generate:
    uses: mirrajabi/aider-github-action/.github/workflows/aider-issue-to-pr.yml@main
    # Check if the label is 'aider'
    if: github.event.label.name == 'aider'
    with:
      issue-number: ${{ github.event.issue.number }}
      base-branch: ${{ github.event.repository.default_branch }}
    secrets: 
      openai_api_key: ${{ secrets.OPENAI_API_KEY }}

```

## Inputs

You can pass the following arguments to the workflow:

| Field Name     | Description                                                   | Required | Type    | Default                  |
|-----------------|---------------------------------------------------------------|----------|---------|--------------------------|
| `base-branch`     | Base branch to create PR against                               | **true**     | string  | -                        |
| `chat-timeout`    | Timeout for chat in *minutes* to prevent burning your credits    | false    | number  | `10`                       |
| `issue-number`    | Issue number                                                  | **true**     | number  | -                        |
| `model`           | Model to use                                                  | false    | string  | `'gpt-4-1106-preview'`    |

## Example

In [aider-github-workflows-test](https://github.com/mirrajabi/aider-github-workflows-test/issues) project you can find the following which demonstrate the full flow from Issue to generated PR.

1. [An issue](https://github.com/mirrajabi/aider-github-workflows-test/issues/1) that triggers the workflow.
1. [The workflow run](https://github.com/mirrajabi/aider-github-workflows-test/actions/runs/7365081934)
1. [A Pull request](https://github.com/mirrajabi/aider-github-workflows-test/pull/2) that was created

## Roadmap

- [ ] Add support for in PR chat and revisions

## Disclaimer

You are responsible for the AI credit that is spent. All contributors in a GitHub project will be able to label issues which will trigger the workflow. Non-contributors are not be able to tag issues which helps greatly in preventing abuse.
