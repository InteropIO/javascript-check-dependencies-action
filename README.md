# JavaScript Lockfile Check

![Tests](https://github.com/ChromaticHQ/javascript-lockfile-check-action/workflows/Tests/badge.svg) ![Linting & Checks](https://github.com/ChromaticHQ/javascript-lockfile-check-action/workflows/Linting%20&%20Checks/badge.svg)

This action checks that the correct lockfile is present at the root of a
project. It also checks that no extraneous lockfiles for other JavaScript
package managers are present. The intent is to enforce a given package manager
and avoid a scenario where more than one lockfile is present.

## Configuration options

- **`rules_url` (required):** URL pointing to a JSON file with the banned package versions:

```json
{
  "@acme/bad": ["1.0.*", "^1.1.2"],
  "evil-package": "*"
}
```

## Usage

Install node and the dependencies, then run.

```yaml
name: Check package-lock for compromised dependencies

on:
  push:
    branches: [ main ]
  pull_request:

jobs:
  check-lock:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout
        uses: actions/checkout@v4

      - name: Use Node
        uses: actions/setup-node@v4
        with:
          node-version: 20

      - name: Install dependencies for custom action
        run: |
          cd .github/actions/check-lock
          npm install

      - name: Run package-lock check
        uses: ./.github/actions/check-lock
        with:
          rules_url: https://example.com/bad-deps.json
```
