# setup-github-token

[![workflows](https://github.com/hostwithquantum/setup-github-token/actions/workflows/check.yml/badge.svg)](https://github.com/hostwithquantum/setup-github-token/actions/workflows/check.yml)

A GitHub Action that creates a GitHub App installation token and resolves the bot's committer identity.

Wraps [`actions/create-github-app-token`](https://github.com/actions/create-github-app-token) and adds `committer-name` and `committer-email` outputs so commits made by your app show up with the correct bot avatar.

## Usage

```yaml
steps:
  - id: token
    uses: hostwithquantum/setup-github-token@v1.0.0
    with:
      client-id: ${{ secrets.APP_CLIENT_ID }}
      private-key: ${{ secrets.APP_PRIVATE_KEY }}

  - uses: actions/checkout@v6
    with:
      token: ${{ steps.token.outputs.token }}

  - run: |
      git config user.name "${{ steps.token.outputs.committer-name }}"
      git config user.email "${{ steps.token.outputs.committer-email }}"
      git commit --allow-empty -m "test"
```

## Inputs

| Name | Required | Default | Description |
|------|----------|---------|-------------|
| `client-id` | yes | | GitHub App client ID |
| `private-key` | yes | | GitHub App private key |
| `owner` | no | `github.repository_owner` | Installation owner, passed to `actions/create-github-app-token` |
| `repositories` | no | `""` | Comma-separated list of repositories to scope the token to |

## Outputs

| Name | Description | Example |
|------|-------------|---------|
| `token` | GitHub App installation token | |
| `committer-name` | Bot committer name | `my-app[bot]` |
| `committer-email` | Bot committer email | `12345+my-app[bot]@users.noreply.github.com` |

## License

See [LICENSE](LICENSE).
