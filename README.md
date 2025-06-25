# API Protector GitHub Action

The API Protector GitHub Action allows you to integrate API specification comparison directly into your CI/CD workflows. It compares two OpenAPI Specification files (JSON/YAML) and reports differences and compatibility, helping you identify breaking changes early.

## Usage

To use the API Protector GitHub Action, you'll need to include it in your GitHub Actions workflow file (`.github/workflows/*.yml`).

### Example Workflow

Here's an example of how to use the `APIProtectorACTION` in your workflow:

```yaml
name: API Diff Check

on: [push, pull_request]

jobs:
  api-diff:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout repository
        uses: actions/checkout@v4

      - name: Create example spec files (for demonstration)
        run: |
          echo '{"openapi": "3.0.0", "info": {"title": "Old Petstore API", "version": "1.0.0"}, "paths": {}}' > petstore2.json
          echo '{"openapi": "3.0.0", "info": {"title": "New Petstore API", "version": "1.0.1"}, "paths": {"/pets": {"get": {"summary": "List all pets"}}}}' > petstore2_changes.json

      - name: Run API Protector Diff
        uses: APIprotector/APIProtectorACTION@main
        with:
          old-spec-file: ./petstore2.json
          new-spec-file: ./petstore2_changes.json
          output-file: 'api-diff-output.txt'
          exit-on-error: true
          verbose: true

      - name: Upload API Diff Output (optional)
        uses: actions/upload-artifact@v4
        with:
          name: api-diff-report
          path: api-diff-output.txt
```

### Inputs

The following inputs can be used to configure the API Protector Action:

| Input             | Description                                                            | Required | Type      | Default   |
| :---------------- | :--------------------------------------------------------------------- | :------- | :-------- | :-------- |
| `old-spec-file`   | Path to the old API specification file (YAML or JSON).                 | `true`   | `string`  |           |
| `new-spec-file`   | Path to the new API specification file (YAML or JSON).                 | `true`   | `string`  |           |
| `output-file`     | Optional path to write the diff output.                                | `false`  | `string`  |           |
| `exit-on-error`   | Exit with error code if the OpenAPI changes are not compatible.        | `false`  | `boolean` | `'false'` |
| `silent`          | Print no output to console.                                            | `false`  | `boolean` | `'false'` |
| `verbose`         | Enable verbose output.                                                 | `false`  | `boolean` | `'false'` |

### Docker Image

This action is powered by the `piachsecki/apiprotector-cli` Docker image. You can find more information about the CLI tool and its versions on Docker Hub:

[https://hub.docker.com/r/piachsecki/apiprotector-cli/tags](https://hub.docker.com/r/piachsecki/apiprotector-cli/tags)
