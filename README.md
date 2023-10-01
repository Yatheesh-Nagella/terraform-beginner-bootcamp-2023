# Terraform Beginner Bootcamp 2023

## Semantic Versioning

This project is going to utilize semantic versioning for its tagging.
[semver.org](https://semver.org/)

The general format:

**MAJOR.MINOR.PATCH**, eg. `1.0.1`

- **MAJOR** version when you make incompatible API changes
- **MINOR** version when you add functionality in a backward compatible manner
- **PATCH** version when you make backward compatible bug fixes

## Install the Terraform CLI
[Install TF CLI](https://developer.hashicorp.com/terraform/tutorials/aws-get-started/install-cli)

## Shebang - Refactoring into bash scripts
Interpreter directives allow scripts and data files to be used as commands, hiding the details of their implementation from users and other programs, by removing the need to prefix scripts with their interpreter on the command line.
[Read more of shebang](https://en.wikipedia.org/wiki/Shebang_(Unix))

## Chmod
Used to change the permissions accesing the code files from shell.
[Read more of Chmod](https://en.wikipedia.org/wiki/Chmod)

### Considerations for Linux Distribution

This project is built against Ubunutu.
Please consider checking your Linux Distrubtion and change accordingly to distrubtion needs. 

[How To Check OS Version in Linux](
https://www.cyberciti.biz/faq/how-to-check-os-version-in-linux-command-line/)

Example of checking OS Version:

```
$ cat /etc/os-release

PRETTY_NAME="Ubuntu 22.04.3 LTS"
NAME="Ubuntu"
VERSION_ID="22.04"
VERSION="22.04.3 LTS (Jammy Jellyfish)"
VERSION_CODENAME=jammy
ID=ubuntu
ID_LIKE=debian
HOME_URL="https://www.ubuntu.com/"
SUPPORT_URL="https://help.ubuntu.com/"
BUG_REPORT_URL="https://bugs.launchpad.net/ubuntu/"
PRIVACY_POLICY_URL="https://www.ubuntu.com/legal/terms-and-policies/privacy-policy"
UBUNTU_CODENAME=jammy
```

#### Execution Considerations

When executing the bash script we can use the `./` shorthand notiation to execute the bash script.

eg. `./bin/install_terraform_cli`

If we are using a script in .gitpod.yml  we need to point the script to a program to interpert it.

eg. `source ./bin/install_terraform_cli`

#### Linux Permissions Considerations

In order to make our bash scripts executable we need to change linux permission for the fix to be exetuable at the user mode.

```sh
chmod u+x ./bin/install_terraform_cli
```

alternatively:

```sh
chmod 744 ./bin/install_terraform_cli
```

https://en.wikipedia.org/wiki/Chmod

### Github Lifecycle (Before, Init, Command)

We need to be careful when using the Init because it will not rerun if we restart an existing workspace.

https://www.gitpod.io/docs/configure/workspaces/tasks


### Working with ENV Vars

We can list out all env vars (Env Vars) using the `env` command

We can filter specific env vars using grep eg. `env | grep AWS_`

#### Setting and Unsetting Env Vars

In the terminal we can set using `export HELLO=`world`

In the terminal we can unset using `unset HELLO`

We can set an env var temporarily when just running a command

```
HELLO='world' ./bin/print_message
```

Within a bash script we can set env without writing export eg.

```
#!/usr/bin/env bash

HELLO= world

echo $HELLO
```

#### Printing Vars

We can print an env var using echo eg. `echo $HELLO`

#### Scoping of Env Vars

When you open up new bash terminals in VSCode it will not be aware of env vars that you have set in another window.

If you want to Env Vars to persist across all future bash terminals that are open you need to set env vars in your bash profile. eg. `.bash_profile`

#### Persisting Env Vars in Gitpod

We can persist env vars into gitpod by storing them in Gitpod Secrets Storage.

```
gp env HELLO='world'
```

All future workspaces launched will set the env vars for all bash terminals opened in thoes workspaces.

You can also set en vars in the `.gitpod.yml` but this can only contain non-senstive env vars.

### AWS CLI Installation

AWS CLI is installed for the project via the bash script [`./bin/install_aws_cli`](./bin/install_aws_cli)

[Getting started with AWS CLI](https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html)

TO check caller identity in AWS:

```sh
aws sts get-caller-identity
```

To configure aws creds:
```sh
aws configure
```

### Terraform 

[Terraform registry](https://registry.terraform.io/)

- **Providers** are a logical abstraction of an upstream API. They are responsible for understanding API interactions and exposing resources.
- **Modules** are small, reusable Terraform configurations that let you manage a group of related resources as if they were a single resource

#### TF init

```sh
terraform init
```
This command initializes a new or existing Terraform configuration. It downloads the necessary provider plugins and sets up the backend, which is where Terraform stores its state.

#### TF plan

```sh
terraform plan
```

This command creates an execution plan based on the Terraform configuration. It shows you what actions Terraform will take to reach the desired state specified in your configuration.

#### TF apply

```sh
terraform apply
```
This command applies the changes described in the execution plan. It prompts you to confirm that you want to make the changes.
To approve automatically : `terraform apply --auto-approve`

#### TF state file

The state file reflects the current state of your infrastructure

#### TF state file

The lock file ensures that operations are performed safely and prevent conflicts in concurrent scenario


```sh
terraform output random_bucket_name
```


