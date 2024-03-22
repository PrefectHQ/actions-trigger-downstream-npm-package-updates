# actions-trigger-downstream-npm-package-updates
Generalized job used to optionally trigger downstream workflows for UI components
## Environment Variables/Secrets

| Env Var/Secret | Desription | Required |
|-------|------------|----------|
| DOWNSTREAM_REPO_NAME | Name of the repo to run the update GHA workflow | true |
| REPO_ACTION_RW_TOKEN | Secret/Env Var containing the Github API key to trigger downstream jobs | true |

## Usage
```yaml
name: Release
on: [ main ]
jobs:
  release_component:
    name: Release
    runs-on: ubuntu-latest

    steps:
      - uses: actions/checkout@v4

      - uses: prefecthq/actions-release-ui-components@main
        id: release-ui-components
        with:
          NPM_TOKEN: ${{ secrets.NPM_TOKEN_SUPER_SECRET }}

      - uses: prefecthq/actions-trigger-downstream-npm-package-updates@main
        id: update_nebula_ui
        if: ${{ steps.release-ui-components.publish.outputs.type }}
        with:
          REPO_ACTION_RW_TOKEN: ${{ secrets.NEBULA_UI_ACTIONS_RW }}
          DOWNSTREAM_REPO_NAME: nebula-ui
```