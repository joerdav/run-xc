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
      env:
        IN: some_input
      with:
        task: "test"
```

## Options

### `version`

The [xc](https://xcfile.dev) version to setup. Must be a valid semantic version string like `v0.0.1` or `latest`.

The default is `latest`.

[joerdav/setup-xc](https://github.com/joerdav/setup-xc) will be used to install this version of [xc](https://xcfile.dev).

### `task`

The name of the [xc](https://xcfile.dev) task to run.

### `env`

Use github action environment variables to define xc task inputs.

## Tasks

### test

inputs: IN

Task used to test this action.

```
echo $IN
```

### tag

Deploys a new tag for the repo.

Specify major/minor/patch with VERSION

inputs: VERSION

```
# https://github.com/unegma/bash-functions/blob/main/update.sh

CURRENT_VERSION=`git describe --abbrev=0 --tags 2>/dev/null`
CURRENT_VERSION_PARTS=(${CURRENT_VERSION//./ })
VNUM1=${CURRENT_VERSION_PARTS[0]}
VNUM2=${CURRENT_VERSION_PARTS[1]}
VNUM3=${CURRENT_VERSION_PARTS[2]}

if [[ $VERSION == 'major' ]]
then
  VNUM1=$((VNUM1+1))
  VNUM2=0
  VNUM3=0
elif [[ $VERSION == 'minor' ]]
then
  VNUM2=$((VNUM2+1))
  VNUM3=0
elif [[ $VERSION == 'patch' ]]
then
  VNUM3=$((VNUM3+1))
else
  echo "Invalid version"
  exit 1
fi

NEW_TAG="v$VNUM1.$VNUM2.$VNUM3"

echo Adding git tag with version ${NEW_TAG}
git tag ${NEW_TAG}
git push origin ${NEW_TAG}
```

