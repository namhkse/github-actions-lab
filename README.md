# Learing Github Actions

## Working with YAML files

YAML is a program language.
File extensions: `.yml` or `.yaml`
Strict: indent, dash, colon, special characters

Use modern editor: nevom, vscode

## Your first action

Create a action in `Actions` tab.

Each job has may steps

## Workflow and action attributes

name: <name>
- The name of the workflow
- Not required

on: <event>
- The events trigger the workflow
- Required

Repo events:
- push
- pull_request
- release
- workflow_dispatch
- webhooks (branch creation, issues, member, schedule)

jobs:
- Workflows must have at least one job
- Must have identifer
- Must start with letter or '_'

runs_on: 
- The type of machine aka runner

Avaiable runner:
- windows server
- ubuntu
- macos
- selfhost

steps:
List of actions or command
Access the runner filesystem
Each step runs in its own process

```yml
steps:
- name: Checkout
  uses: actions/checkout@v4 
- run: echo Hello, world!
```

uses:
Identifiers an action to use
Defines the location of that action

run:
Runs commands in the virtual env shell

name:
an optional identifer for the step

## 1. Actions and workflows

### Create a workflow

### Add jobs and steps to an workflow

Jobs run in parallel
Steps are where we get to define the actions

### Adding actions and command

Action `uses:` execute packaged code on the runner
Command `run:` execute commands in teh runner's default shell
- bash: ubuntu, macos
- powershell: windows

| Action location | Syntax | Example |
| Public repo | uses {owner}/{repo}@{ref} | uses: actions/checkout@v4 |
| Same repo | uses: ./path/to/the/action | uses: ./scrips/my-local-action |
| Docker registry | uses: docker:://{image}:{tag} | uses: docker://hello-world:latest |

Checkout actions:
- uses: actions/checkout@v4

Setup compilers actions:
- uses: actions/go@v5.5
- uses: actions/node@v5.5
- and more

Adding a command
- Single-line
- Multi-lines (start with pile)

### Run a workflow

TODO: add my thinking 

### Adding dependencies

Jobs run in paralel, how to run one by one

```yml
    job1:
    job2: 
    job3:
```

Use `need
```yml
    job1:
    job2: 
    job3:
        need: [job1, job2]
```

### Workflow trigger

Trigger when pushing on main and develop.

```yml
on:
  push:
    branches:
      - main
      - develop
```

This is perfect for one trigger.
```yml
on:
  pull_request:
    branch:
      - main
```

Ignore branches or tags

Only use one in a workflow

### Workflow and action litmis

Workflow
- 20 concurrent wofkflows per repo

Jobs
- 20 concurrent job runs per repo
- Max lifetime is 6h

Actions:
- 1000 github api request per hour
- Actions can't trigger other workflow
- Logs are limited to 64KB

Exceeding limits cause jobs are killed.

## Selecting and using actions

### Use an action from a repo

- Actions in the workflow's repo
- Public repo
- Docker repo

### Pass arguments to an action

Seteps use the "with" attribute to pass arguments

Create a new block for mapping arguments to inputs

```yml
- name: the-step-name
  uses: {github_account}/{action_name}
  with:
      key:value
      key:value
```

### Enviroment variables

Dynamic key-value pairs stored in memory
Injected at runtime

Defineing env variables
Scope:
- Workflow
- Job
- Step

Accessing env variable
- Shell syntax $ENV_NAME or $Env:ENV_NAME
- YAML syntax ${{ env.VARIABLE_NAME }}

### Artifacts

Anything preserved from a workflow 
- compiled binaries
- archie
- log files

Pass data between workflow jobs

Job1: create and upload an artifact
Job2: Wait for job 1 to complete and download the artifact 

uses actions/upload-artifact
uses: actions/download-artifiact

Limit
Free account: size less that 50MB 