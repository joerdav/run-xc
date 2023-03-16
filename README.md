# run-xc action.

## Usage

```yml
name: "CI"
on: ["push", "pull_request"]

jobs:
  ci:
    name: "Run CI"
    runs-on: ubuntu-latest
    steps:
    - uses: actions/checkout@v2
      with:
        fetch-depth: 1
    - uses: joerdav/run-xc@v0.0.1
      with:
        task: "test"
```

## Tasks

### test

inputs: IN

Task used to test this action.

```
echo $IN
```
