<div align="center">

![license](https://img.shields.io/github/license/seaofvoices/test-luau-package-action)

[![ko-fi](https://ko-fi.com/img/githubbutton_sm.svg)](https://ko-fi.com/seaofvoices)

</div>

# Test Luau Package Action

A GitHub Action to simplify and unify test workflows across Sea of Voices Luau projects. These projects follow the [Sea of Voices Luau Package Standard](https://github.com/seaofvoices/luau-package-standard).

This action will initialize the project by running these steps:

- Check out the repository (skip this step by setting `skip-checkout` to `true` if the job has already checked out the repository)
- Install tools using [Rokit](https://github.com/rojo-rbx/rokit)
- Setup Node
- Install packages (`yarn install --immutable` or `npm ci`)

Then, make sure the `package.json` file contains the following [scripts](https://docs.npmjs.com/cli/v11/using-npm/scripts) which will run in this order:

- `prepare`: runs [`npmluau`](https://github.com/seaofvoices/npmluau)
- `lint`: runs static analysis like [`luau-analyze`](https://github.com/JohnnyMorganz/luau-lsp) and [`selene`](https://github.com/Kampfkarren/selene)
- `style-check`: runs [`stylua`](https://github.com/JohnnyMorganz/StyLua)
- `build`: runs [`darklua`](https://github.com/seaofvoices/darklua) and/or [`rojo`](https://github.com/rojo-rbx/rojo) to build artifacts

These scripts match the Luau packages on [Sea of Voices](https://github.com/seaofvoices). This project setup can be created easily using the [`generator-luau`](https://github.com/seaofvoices/generator-luau) tool.

Add this action to your workflow by referencing it in your `.github/workflows/test.yml` file:

```yaml
name: Tests

"on":
  push:
    branches:
      - main
  pull_request:
    branches:
      - main

jobs:
  test:
    name: Verify
    runs-on: ubuntu-latest
    steps:
      - uses: seaofvoices/test-luau-package-action@v1
        with:
          package-manager: yarn # or npm

          # if the job already has the repository, skip the checkout
          # skip-checkout: true

          # if package.json is not at the repo root (path is relative to the checkout root)
          # working-directory: packages/my-package
```

## Inputs

Boolean inputs use the string `true` or `false` (YAML booleans are accepted and behave the same).

| Input | Description | Required | Default |
|-------|-------------|----------|---------|
| `package-manager` | Package manager to use (`yarn` or `npm`) | No | `'yarn'` |
| `skip-checkout` | Skip the checkout step (use only if a previous step already checked out the repository) | No | `'false'` |
| `skip-build` | Skip the `build` script step | No | `'false'` |
| `skip-format` | Skip the `style-check` script step | No | `'false'` |
| `skip-lint` | Skip the `lint` script step | No | `'false'` |
| `working-directory` | Directory for Corepack and all `npm` / `yarn` steps, relative to the repository root; omit to use the repository root | No | `.` |

## License

This project is available under the MIT license. See [LICENSE.txt](LICENSE.txt) for details.
