---
description: Instrument services that run from CI and generate reports with insights
---

# Install in CI

When running tests as part CI workflow, Aspecto can compare a run to a previous baseline and generate a report with insights about changes and possible issues introduced into the system by a change in the codebase.

To mark a service as running in CI context, set the environment variable `ASPECTO_CI_REPORT` to `1`, or use the `ciReport: true` when calling the `instrument` function. 

Additionally, Aspecto needs to detect the git SHA value of the commit, and the branch name. These will be auto detected from the `.git` folder, or from relevant CI environment variables. If your run your service in a conatainer, pass those variables into the container

### Github Actions

`docker run -e ASPECTO_CI_REPORT=1 -e GITHUB_HEAD_REF -e GITHUB_REF -e GITHUB_SHA` 

### CircleCI

`docker run -e ASPECTO_CI_REPORT=1 -e CIRCLE_BRANCH -e CIRCLE_SHA1` 

## Running from docker-compose

If you run your services in CI with docker-compose, it is recommended to add the relevant environment variables to the `docker-compose.yml`file, and set `ASPECTO_CI_REPORT=1` in your CI workflow:

```text
services:
    my_serivce:
        image: **set your image here**
        environment:
          - ASPECTO_CI_REPORT
          
          # Github Actions:
          - GITHUB_HEAD_REF
          - GITHUB_REF
          - GITHUB_SHA
          
          # CircleCI:
          - CIRCLE_BRANCH
          - CIRCLE_SHA1 
```

And from your workflow:

```text
jobs:
  test:
    steps:
      - name: Setup service for CI tests
        run: docker-compose up -d
        env: 
          ASPECTO_CI_REPORT: 1

```

