# EvenNode Deploy Action

GitHub Action for seamless deployment to [EvenNode](https://www.evennode.com) platforms.

> [!NOTE]
> This action requires an `EvenNode repository URL` and `SSH key` authorization.
>   
> If you're not familiar with these, please refer to the documentation about deploying an EvenNode application and generating an SSH key for upload:
> https://www.evennode.com/docs/git-deployment

## Features

- Secure SSH key handling
- Automatic .env file configuration
- Custom pre-commit and pre-push commands
- Flexible branch targeting
- Force push control

## Basic Usage

```yaml
name: Deploy to EvenNode

on:
  push:
    branches: [ main ]

jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      
      - name: Deploy to EvenNode
        uses: your-username/evennode-deploy-action@v1
        with:
          key: ${{ secrets.EVENNODE_SSH_KEY }}
          git_email: 'john@example.com'
          git_name: 'John Peterson'
          git_url: 'git@git.evennode.com:the-repo.git'
          dot_env: ${{ secrets.DOT_ENV }}
```

## Recommended Usage

```yaml
name: Deploy to EvenNode

on:
  push:
    branches: [ main ]

jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      
      - name: Deploy to EvenNode
        uses: your-username/evennode-deploy-action@v1
        with:
          key: ${{ secrets.EVENNODE_SSH_KEY }}
          git_email: ${{ secrets.GIT_EMAIL }}
          git_name: ${{ secrets.GIT_NAME }}
          git_url: ${{ secrets.EVENNODE_GIT_URL }}
          dot_env: ${{ secrets.DOT_ENV }}
```

**Why use secrets for credentials?**
- Allows different team members to use their own credentials
- Enables easy rotation of credentials without code changes

## Inputs

| Input | Required | Description |
|-------|----------|-------------|
| `key` | Yes | SSH private key for EvenNode |
| `git_email` | Yes | Git email for commits |
| `git_name` | Yes | Git username for commits |
| `git_url` | Yes | EvenNode Git repository URL |
| `dot_env` | No | Content for .env file |
| `commit_message` | No | Custom commit message |
| `branch` | No | Target branch (default: main) |
| `pre_commit_command` | No | Command to run before commit (e.g. build commands or adding build artifacts) |
| `pre_push_command` | No | Command to run before push |
| `force_push` | No | Force push (default: true) |

## About pre_commit_command

The `pre_commit_command` input allows you to execute custom commands before the deployment commit is created. This is particularly useful for:

1. Building your application and including the build artifacts in the commit
2. Running tests or validations before deployment
3. Preparing any necessary files or directories

A example use case is building a Next.js application and including the `.next` folder in the commit to avoid recompilation on the EvenNode host. Here's an example from a real implementation:

```yaml
pre_commit_command: |
  npm install
  npm run build
  git add .next
```

You can see this in action in [this workflow.](https://github.com/rodnye/inf-cujae-website/blob/8c56335a5fba293824efecab8737e5808814871c/.github/workflows/evennode.yml#L43)

This approach significantly reduces deployment time as the host doesn't need to rebuild the application. The built files are included directly in the deployment.
