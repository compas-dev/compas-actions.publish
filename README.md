# compas-actions.publish

Publish COMPAS or its plugins to PyPI and create a GitHub release.

## Trusted Publishing

When `pypi_token` is omitted, the action publishes with PyPI Trusted
Publishing. The calling job must grant GitHub's OIDC token permission:

```yaml
jobs:
  release:
    runs-on: ubuntu-latest
    environment: pypi
    permissions:
      contents: write
      id-token: write
    steps:
      - uses: compas-dev/compas-actions.publish@v3
        with:
          github_token: ${{ secrets.GITHUB_TOKEN }}
```

Before using this configuration, add a GitHub Actions trusted publisher to the
project on PyPI. Configure it with the owner, repository, and workflow filename
of the repository that calls this action. If the publisher is restricted to a
GitHub environment, its name must match `environment` in the calling job.

The `id-token: write` permission cannot be declared by this composite action;
it must be set on the calling job or workflow. An environment is optional, but
recommended. If `create_release` is enabled (the default), the job also needs
`contents: write`.

## API token publishing

Existing token-based workflows remain supported. Passing a non-empty
`pypi_token` selects the API token upload path and does not require
`id-token: write`:

```yaml
jobs:
  release:
    runs-on: ubuntu-latest
    permissions:
      contents: write
    steps:
      - uses: compas-dev/compas-actions.publish@v3
        with:
          github_token: ${{ secrets.GITHUB_TOKEN }}
          pypi_token: ${{ secrets.PYPI_TOKEN }}
```
