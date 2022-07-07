<!-- start title -->

# GitHub Action:Run Terraform

<!-- end title -->
<!-- start description -->

Runs a specified terraform command and comments on pull requests. Has built in
support for authenticating to AWS.

All commands execute a `terraform init`.

The `plan` command also executes the `terraform fmt` and `terraform validate`
commands are reports their result in the PR comment.

The `validate` command is meant for when you only want to run a `terraform
validate` command, for use in terraform modules that have limited testing
capability without running integration tests. There is no need to run the
action twice with a `validate` and `plan` command.

<!-- end description -->
<!-- start contents -->
<!-- end contents -->
<!-- start usage -->

```yaml
- uses: catalystsquad/action-terraform@undefined
  with:
    # Which Terraform command to execute, supports `plan` and `apply`
    command: ""

    # Location of the Terraform root module to execute from
    # Default: ./
    work-dir: ""

    # Check format with `terraform fmt`, report errors in PR comment
    # Default: true
    check-format: ""

    # Validate configuration with `terraform validate`, report errors in PR comment
    # Default: true
    check-validate: ""

    # Whether to comment the command's result on the pull request if the event is a
    # pull request
    # Default: true
    comment-on-pr: ""

    # Github token to use for creating comments
    # Default: ${{ github.token }}
    github-token: ""

    # Cloud provider to get credentials for. Currently only supports `aws`.
    # Default:
    provider: ""

    # AWS region
    # Default: us-west-2
    aws-region: ""

    # AWS access key id to use
    # Default:
    aws-access-key-id: ""

    # AWS secret access key
    # Default:
    aws-secret-access-key: ""
```

<!-- end usage -->
<!-- start inputs -->

| **Input**                   | **Description**                                                                            |      **Default**      | **Required** |
| :-------------------------- | :----------------------------------------------------------------------------------------- | :-------------------: | :----------: |
| **`command`**               | Which Terraform command to execute, supports `plan` and `apply`                            |                       |   **true**   |
| **`work-dir`**              | Location of the Terraform root module to execute from                                      |         `./`          |  **false**   |
| **`check-format`**          | Check format with `terraform fmt`, report errors in PR comment                             |        `true`         |  **false**   |
| **`check-validate`**        | Validate configuration with `terraform validate`, report errors in PR comment              |        `true`         |  **false**   |
| **`comment-on-pr`**         | Whether to comment the command's result on the pull request if the event is a pull request |        `true`         |  **false**   |
| **`github-token`**          | Github token to use for creating comments                                                  | `${{ github.token }}` |  **false**   |
| **`provider`**              | Cloud provider to get credentials for. Currently only supports `aws`.                      |                       |  **false**   |
| **`aws-region`**            | AWS region                                                                                 |      `us-west-2`      |  **false**   |
| **`aws-access-key-id`**     | AWS access key id to use                                                                   |                       |  **false**   |
| **`aws-secret-access-key`** | AWS secret access key                                                                      |                       |  **false**   |

<!-- end inputs -->
<!-- start outputs -->

| **Output**      | **Description** | **Default** | **Required** |
| :-------------- | :-------------- | ----------- | ------------ |
| `random-number` | Random number   |             |              |

<!-- end outputs -->
<!-- start examples -->

### Example usage

Terraform Plan:
```yaml
name: Validate pull request
on:
  pull_request:
    branches:
      - main
jobs:
  plan:
    name: Plan
    runs-on: ubuntu-latest
    steps:
      - name: Plan
        uses: catalystsquad/action-terraform@v1
        with:
          command: plan
          work-dir: my-tf-env
          provider: aws
          aws-region: us-west-2
          aws-access-key-id: ${{ secrets.TERRAFORM_AWS_ACCESS_KEY_ID }}
          aws-secret-access-key: ${{ secrets.TERRAFORM_AWS_SECRET_ACCESS_KEY }}
```

Terraform Apply:
```yaml
name: Deploy
on: # only trigger on prs to main closed
  pull_request:
    types:
      - closed
    branches:
      - main
jobs:
  apply:
    if: github.event.pull_request.merged == true # only deploy on merged PRs
    name: Apply
    runs-on: ubuntu-latest
    steps:
      - name: Apply
        uses: catalystsquad/action-terraform@v1
        with:
          command: apply
          work-dir: my-tf-env
          provider: aws
          aws-region: us-west-2
          aws-access-key-id: ${{ secrets.TERRAFORM_AWS_ACCESS_KEY_ID }}
          aws-secret-access-key: ${{ secrets.TERRAFORM_AWS_SECRET_ACCESS_KEY }}
```

<!-- end examples -->
<!-- start [.github/ghdocs/examples/] -->
<!-- end [.github/ghdocs/examples/] -->
