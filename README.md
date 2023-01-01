# git-repository

Project to create a new Github repository, and configure and protect its default branches. It can be used both as a Python package, as a CLI command, and as a GitHub Action.

## How it works

After sanitize provided data, it creates a new Github repository inside the organization, and add it a `LICENSE` and `README.md` files based on some templates. Later it adjusts project branches based on the `repository_purpose` field, and apply the repo and branches protection rules. Finally, it creates a new team in the organization specific for that particular project, and adds the new repository to that team.

## How to use

As a Github Action, use the action in your workflow file:

```yaml
uses: OneCloudAutomation/git-release@v1
with:
  token: ${{ secrets.GITHUB_TOKEN }}
  action_type: create
  repository_purpose: trunk
  application_name: my-app
```

As a CLI command, use the following commands to download and run the tool locally:

```shell
git clone https://github.com/OneCloudAutomation/git-repository.git
cd git-repository
go mod vendor
go run .
```

## Inputs

Name               | Description                                                     | Default   | Required
:----------------- | :-------------------------------------------------------------- | :-------: | :-------:
token              | string with the Github personal access token                    |           | `Yes`
action_type        | string with the action type (`create`, `patch` or `update`)     |           | `Yes`
repository_purpose | string with the repository purpose type (`trunk` or `deploy`)|           | `Yes`
application_name   | string with the application name                                |           | `Yes`
environment_name   | string with the environment name                                | `dcs`     | `No`
organization_name  | string with the organization name                               | `OneCloudAutomation`| `Yes`
branch_names     | list of strings with the names of the branches to be protected  | based on value of `repository_purpose`: `main` for `trunk` purpose, and `development`, `non-production`, `production` for `deploy` purpose | `No`
team_parent_id              | string with the team parent ID	                    |           | `No`


> When used as a function, it returns a tuple with the created `repo`, `branches` and `team` objects.

## Outputs
Name         | Description                                                     
:----------- | :------------------------------------------- 
repo_url     | URL of the repository
branch_names | list of base protected branches in the repository
team_url     | URL of the Github team accessing to the repository

## Test and run the action locally

You can run the action locally using the [act](https://github.com/nektos/act) tool.

`act` runs workflows in the `.github/worflows` directory of the repository.

Rename `.env.example` and `.secrets.example` files to `.env` and `.secrets`, respectively.

Write down an appropriate GitHub access token in the `.secrets` file.

`act` will load environment variables and secrets defined in the `.env` and `.secrets` files before running.

`act` configuration flags are provided in the `.actrc` file. Any flags in this file will be applied before any flags provided directly on the command line.

You can run `act` in the root directory of the repository:
```shell
  $ act
```
If you want to rebuild the docker image of the github action:
```shell
  $ act --rebuild
```
In addition, you can run a specific job:
```shell
  $ act --rebuild -j <job_name>
```

The `.github/workflows/ci.yaml` workflow runs the unit tests in the `app_repo/tests` directory, runs this a action for functional testing and generates a new Github release if pushing to the 'main' branch.

## Testing

Run the following commands to run the unit tests :

```shell
  $ cd app_repo/tests
  $ ./mock_function.sh
  $ go test -v
```

<div style="padding:100% 0 0 0;position:relative;"><iframe src="https://player.vimeo.com/video/785601323?h=f2c14deaf5&amp;badge=0&amp;autopause=0&amp;player_id=0&amp;app_id=58479" frameborder="0" allow="autoplay; fullscreen; picture-in-picture" allowfullscreen style="position:absolute;top:0;left:0;width:100%;height:100%;" title="Rustic Filter"></iframe></div><script src="https://player.vimeo.com/api/player.js"></script>
