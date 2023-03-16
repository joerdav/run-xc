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

### tag

Deploys a new tag for the repo.

Specify major/minor/patch with VERSION

Inputs: VERSION

Requires: test

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

NEW_TAG="$VNUM1.$VNUM2.$VNUM3"

echo Adding git tag with version ${NEW_TAG}
git tag ${NEW_TAG}
git push origin ${NEW_TAG}
```

