# EvenNode Deploy Action

GitHub Action for seamless deployment to EvenNode platforms.

## Features

- Secure SSH key handling
- Automatic .env file configuration
- Custom pre-commit and pre-push commands
- Flexible branch targeting
- Force push control

## Usage

This is a basic usage:
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
| `pre_commit_command` | No | Command to run before commit |
| `pre_push_command` | No | Command to run before push |
| `force_push` | No | Force push (default: true) |