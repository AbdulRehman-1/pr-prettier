# GitHub PR Prettier

A GitHub action to format the changed files using [Prettier](https://prettier.io).

---

## Getting Started

### Parameters

| Name | Required | Default | Description |
|------|----------|---------|-------------|
| `dry` | No | `false` | Execute in dry mode. No files will be modified, and the action fails if unformatted files are found. Recommended with `prettier_options --check`. |
| `prettier_version` | No | `false` | Specify a Prettier version (defaults to the latest). |
| `working_directory` | No | `false` | Directory to change into before installing and running Prettier. Use a relative path, e.g., `app/`. |
| `prettier_options` | No | `"--write **/*.js"` | Prettier options (default applies to the entire repository). |
| `commit_options` | No | - | Custom git commit options. |
| `push_options` | No | - | Custom git push options. |
| `same_commit` | No | `false` | Update the current commit instead of creating a new one. Only works if the checkout action fetches depth '0' (see example 2). |
| `commit_message` | No | `"Prettified Code!"` | Custom git commit message. Ignored if `same_commit` is used. |
| `commit_description` | No | - | Custom extended commit message. Ignored if `same_commit` is used. |
| `file_pattern` | No | `*` | Custom git add file pattern. Cannot be used with `only_changed`. |
| `prettier_plugins` | No | - | Install Prettier plugins, e.g., `"@prettier/plugin-php" "@prettier/plugin-other"`. Must be wrapped in quotes since `@` is a reserved character in YAML. |
| `clean_node_folder` | No | `true` | Delete the `node_modules` folder before committing. |
| `only_changed` | No | `false` | Only format changed files. Cannot be used with `file_pattern`. Only works if the checkout action fetches depth '0' (see example 2). |
| `github_token` | No | `${{ github.token }}` | The default [GITHUB_TOKEN](https://docs.github.com/en/actions/reference/authentication-in-a-workflow#about-the-github_token-secret) or a [Personal Access Token](https://docs.github.com/en/github/authenticating-to-github/keeping-your-account-and-data-secure/creating-a-personal-access-token). |

> **Note:** Using `same_commit` may cause issues if other actions rely on the commit being unchanged before and after this action runs.

---

## Example Workflows

### Basic Usage (On push to the main branch)

```yaml
name: CI

on:
  pull_request:
  push:
    branches:
      - main

jobs:
  format:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout code
        uses: actions/checkout@v3
        with:
          ref: ${{ github.head_ref }}

      - name: Run Prettier
        uses: AbdulRehman-1/pr-prettier@latest
        with:
          prettier_options: --write **/*.{js,md}
```

### Using `only_changed` or `same_commit` on PR

```yaml
name: CI

on:
  pull_request:
    branches: [main]

jobs:
  format:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout code
        uses: actions/checkout@v3
        with:
          ref: ${{ github.head_ref }}
          fetch-depth: 0

      - name: Run Prettier
        uses: AbdulRehman-1/pr-prettier@latest
        with:
          prettier_options: --write **/*.{js,md}
          only_changed: true
```

### Using a Custom Access Token

```yaml
name: CI

on:
  pull_request:
    branches: [main]

jobs:
  format:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout code
        uses: actions/checkout@v3
        with:
          fetch-depth: 0
          ref: ${{ github.head_ref }}
          persist-credentials: false

      - name: Run Prettier
        uses: AbdulRehman-1/pr-prettier@latest
        with:
          prettier_options: --write **/*.{js,md}
          only_changed: true
          github_token: ${{ secrets.PERSONAL_GITHUB_TOKEN }}
```

### Dry Run

```yaml
name: CI

on:
  pull_request:
    branches: [main]

jobs:
  format:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout code
        uses: actions/checkout@v3
        with:
          fetch-depth: 0
          ref: ${{ github.head_ref }}
          persist-credentials: false

      - name: Run Prettier
        uses: AbdulRehman-1/pr-prettier@latest
        with:
          dry: true
          github_token: ${{ secrets.PERSONAL_GITHUB_TOKEN }}
```

More details on workflow syntax can be found in the [GitHub Actions Documentation](https://help.github.com/en/actions/automating-your-workflow-with-github-actions/workflow-syntax-for-github-actions).

---

## Contributing

1. Fork it!
2. Create your feature branch: `git checkout -b my-new-feature`
3. Commit your changes: `git commit -am 'Add some feature'`
4. Push to the branch: `git push origin my-new-feature`
5. Submit a pull request :D

## Infos

- Use `AbdulRehman-1/pr-prettier@latest` to get the latest none development version of the action.

- Please report everything like bugs by creating an [issue](https://github.com/AbdulRehman-1/pr-prettier/issues/new/choose).

## Author

**eslint-action** © [Abdul Rehman](https://github.com/AbdulRehman-1)  
Authored and maintained by Abdul Rehman.
