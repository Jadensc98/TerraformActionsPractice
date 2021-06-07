
NOTES


Didn't realize that Github required a token for login when using HTTPS for repos.

Added the AWS Secret Keys to the repo

Pull requests required before terraform apply/plan: https://github.com/dflook/terraform-github-actions/tree/master/terraform-apply

Example of linting through terraform:

<pre><code>name: Lint
on:
  push:
    branches:
      - '!master'

jobs:
  validate:
    runs-on: ubuntu-latest
    name: Validate terraform configuration
    steps:
      - name: Checkout
        uses: actions/checkout@v2

      - name: terraform validate
        uses: dflook/terraform-validate@v1
        with:
          path: my-terraform-config

  fmt-check:
    runs-on: ubuntu-latest
    name: Check formatting of terraform files
    steps:
      - name: Checkout
        uses: actions/checkout@v2

      - name: terraform fmt
        uses: dflook/terraform-fmt-check@v1
        with:
          path: my-terraform-config
</code></pre>

Checking for drift

This workflow runs every morning and checks that the state of your infrastructure matches the configuration.
This can be used to detect manual or misapplied changes before they become a problem. If there are any unexpected changes, the workflow will fail.
          drift.yaml

          name: Check for infrastructure drift

          on:
            schedule:
              - cron:  "0 8 * * *"

          jobs:
            check_drift:
              runs-on: ubuntu-latest
              name: Check for drift of example terraform configuration
              steps:
                - name: Checkout
                  uses: actions/checkout@v2

                - name: Check for drift
                  uses: dflook/terraform-check@v1
                  with:
                    path: my-terraform-config
