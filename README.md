# create-pr-action

A Github Action which automatically creates PR from one branch (configurable with env variable HEAD_BRANCH) 
against another branch (configurable with env variable as BASE_BRANCH) if it (HEAD_BRANCH) is ahead of another branch (BASE_BRANCH)

Code owner's / reviewers can then merge this PR with usual flow.

## Usecase
This action can be used to keep any two branches in sync. For example, if you have development branch (let's say master or main) 
and release branch (let's say release) then any hotfixes pushed to release branch should also be merged to development branch.
This plugin automatically creates PR from specified HEAD_BRANCH against BASE_BRANCH if HEAD_BRANCH is ahead of BASE_BRANCH.

## Usage

Following example show usage of action to create PR from release (HEAD_BRANCH) against main (BASE_BRANCH)
```yaml
name: Create PR

on:  [push]

jobs:
  release:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2

      - name: Create PR
        uses: medlypharmcy/create-pr-action@master
        env:
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
```

## Options

### Environment variables

1. GITHUB_TOKEN (required) - Required for permission to create PR.
1. HEAD_BRANCH(optional) - Branch to create PR from (default: release)
2. BASE_BRANCH(optional) - Branch to raise PR against (default: main)

Following example shows usage of create pr action which will create PR from development branch against master.
```yaml
name: build_and_upload

on: [push]

jobs:
  release:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2
      - name: Set up JDK 11.0.8

        uses: actions/setup-java@v1
        with:
          java-version: 11.0.8
          
      - name: Build
        run: echo "building project"

      - name: Create PR
        uses: medlypharmacy/create-pr-action@master
        env:
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
          HEAD_BRANCH: development
          BASE_BRANCH: master
```
## Workflow

1. Add this action to your repo.
1. Push changes to HEAD_BRANCH (ex: hotfixes to release branch)
1. This action creates PR from HEAD_BRANCH against BASE_BRANCH. If PR has zero file changes, then PR is auto closed.


## Roadmap for upcoming versions of this action
1. Integrate slack to notify repository owners about PR.
1. Feature for Cross repository PRs.
1. Feature to create PRs against multiple branches

