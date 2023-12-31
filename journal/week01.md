<!-- TOC start  -->

- [Terraform Begineer Bootcamp 2023 - week 01](#terraform-begineer-bootcamp-2023-week-01)
   * [Fixing Tags](#fixing-tags)
   * [Root Module Structure](#root-module-structure)
   * [Terraform cloud Variables](#terraform-cloud-variables)
      + [Loading Terraform Input Variables](#loading-terraform-input-variables)
      + [var flag](#var-flag)
      + [var-file flag](#var-file-flag)
      + [terraform.tvfars](#terraformtvfars)
      + [auto.tfvars](#autotfvars)
      + [order of terraform variables](#order-of-terraform-variables)
   * [Dealing With Configuration Drift](#dealing-with-configuration-drift)
   * [What happens if we lose our state file?](#what-happens-if-we-lose-our-state-file)
      + [Fix Missing Resources with Terraform Import](#fix-missing-resources-with-terraform-import)
      + [Fix Manual Configuration](#fix-manual-configuration)
   * [Fix using Terraform Refresh](#fix-using-terraform-refresh)
   * [Terraform Modules](#terraform-modules)
      + [Terraform Module Structure](#terraform-module-structure)
      + [Passing Input Variables](#passing-input-variables)
      + [Modules Sources](#modules-sources)
   * [Considerations when using ChatGPT to write Terraform](#considerations-when-using-chatgpt-to-write-terraform)
   * [Working with Files in Terraform](#working-with-files-in-terraform)
      + [Fileexists function](#fileexists-function)
      + [Filemd5](#filemd5)
      + [Path Variable](#path-variable)
   * [Terraform Locals](#terraform-locals)
   * [Terraform Data Sources](#terraform-data-sources)
   * [Working with JSON](#working-with-json)
      + [AWS Infra SS](#aws-infra-ss)
      + [Changine the lifecycle ](#changine-the-lifecycle)
   * [Terraform Data](#terraform-data)
   * [Provisioners](#provisioners)
      + [Local-exec](#local-exec)
      + [Remote-exec](#remote-exec)
   * [For Each Expressions](#for-each-expressions)
      + [terraform console](#terraform-console)
      + [Week 1 - Expected output screenshots](#week-1-expected-output-screenshots)

<!-- TOC end -->


<!-- TOC --><a name="terraform-begineer-bootcamp-2023-week-01"></a>
# Terraform Begineer Bootcamp 2023 - week 01

<!-- TOC --><a name="fixing-tags"></a>
## Fixing Tags

[How to Delete Local and Remote Tags on Git](https://devconnected.com/how-to-delete-local-and-remote-tags-on-git/)

Locall delete a tag
```sh
git tag -d <tag_name>
```

Remotely delete tag

```sh
git push --delete origin tagname
```

Checkout the commit that you want to retag. Grab the sha from your Github history.

```sh
git checkout <SHA>
git tag M.M.P
git push --tags
git checkout main
```

<!-- TOC --><a name="root-module-structure"></a>
## Root Module Structure

Our root module structure is as follows:
```
PROJECT_ROOT
│
├── main.tf                 # everything else.
├── variables.tf            # stores the structure of input variables
├── terraform.tfvars        # the data of variables we want to load into our terraform project
├── providers.tf            # defined required providers and their configuration
├── outputs.tf              # stores our outputs
└── README.md               # required for root modules
```

[Standard Module Structure](https://developer.hashicorp.com/terraform/language/modules/develop/structure)


<!-- TOC --><a name="terraform-cloud-variables"></a>
## Terraform cloud Variables

I terraform we can set two kind of vars:
- Env vars - those are AWS creds
- Tf vars - those are tf uuid

<!-- TOC --><a name="loading-terraform-input-variables"></a>
### Loading Terraform Input Variables

[Terraform Input Variables](https://developer.hashicorp.com/terraform/language/values/variables)

<!-- TOC --><a name="var-flag"></a>
### var flag
We can use the `-var` flag to set an input variable or override a variable in the tfvars file eg. `terraform -var user_ud="my-user_id"`

<!-- TOC --><a name="var-file-flag"></a>
### var-file flag

- TODO: document this flag

<!-- TOC --><a name="terraformtvfars"></a>
### terraform.tvfars

This is the default file to load in terraform variables in blunk

<!-- TOC --><a name="autotfvars"></a>
### auto.tfvars

- TODO: document this functionality for terraform cloud

<!-- TOC --><a name="order-of-terraform-variables"></a>
### order of terraform variables

- TODO: document which terraform variables takes presendence.

- [Error](https://github.com/Yatheesh-Nagella/terraform-beginner-bootcamp-2023/issues/19)

<!-- TOC --><a name="dealing-with-configuration-drift"></a>
## Dealing With Configuration Drift

<!-- TOC --><a name="what-happens-if-we-lose-our-state-file"></a>
## What happens if we lose our state file?

If you lose your statefile, you most likley have to tear down all your cloud infrastructure manually.

You can use terraform port but it won't for all cloud resources. You need check the terraform providers documentation for which resources support import.

<!-- TOC --><a name="fix-missing-resources-with-terraform-import"></a>
### Fix Missing Resources with Terraform Import

`terraform state rm aws_s3_bucket.example`

`terraform import aws_s3_bucket.bucket bucket-name`

[Terraform Import](https://developer.hashicorp.com/terraform/cli/import)
[AWS S3 Bucket Import](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/s3_bucket#import)

<!-- TOC --><a name="fix-manual-configuration"></a>
### Fix Manual Configuration

If someone goes and delete or modifies cloud resource manually through ClickOps. 

If we run Terraform plan is with attempt to put our infrstraucture back into the expected state fixing Configuration Drift

<!-- TOC --><a name="fix-using-terraform-refresh"></a>
## Fix using Terraform Refresh

```sh
terraform apply -refresh-only -auto-approve
```

<!-- TOC --><a name="terraform-modules"></a>
## Terraform Modules

<!-- TOC --><a name="terraform-module-structure"></a>
### Terraform Module Structure

It is recommend to place modules in a `modules` directory when locally developing modules but you can name it whatever you like.

<!-- TOC --><a name="passing-input-variables"></a>
### Passing Input Variables

We can pass input variables to our module.
The module has to declare the terraform variables in its own variables.tf

```tf
module "terrahouse_aws" {
  source = "./modules/terrahouse_aws"
  user_uuid = var.user_uuid
  bucket_name = var.bucket_name
}
```

<!-- TOC --><a name="modules-sources"></a>
### Modules Sources

Using the source we can import the module from various places eg:
- locally
- Github
- Terraform Registry

```tf
module "terrahouse_aws" {
  source = "./modules/terrahouse_aws"
}
```


[Modules Sources](https://developer.hashicorp.com/terraform/language/modules/sources)


<!-- TOC --><a name="considerations-when-using-chatgpt-to-write-terraform"></a>
## Considerations when using ChatGPT to write Terraform

LLMs such as ChatGPT may not be trained on the latest documentation or information about Terraform.

It may likely produce older examples that could be deprecated. Often affecting providers.

<!-- TOC --><a name="working-with-files-in-terraform"></a>
## Working with Files in Terraform


<!-- TOC --><a name="fileexists-function"></a>
### Fileexists function

This is a built in terraform function to check the existance of a file.

```tf
condition = fileexists(var.error_html_filepath)
```

https://developer.hashicorp.com/terraform/language/functions/fileexists

<!-- TOC --><a name="filemd5"></a>
### Filemd5

https://developer.hashicorp.com/terraform/language/functions/filemd5

<!-- TOC --><a name="path-variable"></a>
### Path Variable

In terraform there is a special variable called `path` that allows us to reference local paths:
- path.module = get the path for the current module
- path.root = get the path for the root module
[Special Path Variable](https://developer.hashicorp.com/terraform/language/expressions/references#filesystem-and-workspace-info)

```tf
resource "aws_s3_object" "index_html" {
  bucket = aws_s3_bucket.website_bucket.bucket
  key    = "index.html"
  source = "${path.root}/public/index.html"
}
```

<!-- TOC --><a name="terraform-locals"></a>
## Terraform Locals

Locals allows us to define local variables.
It can be very useful when we need transform data into another format and have referenced a varaible.

```tf
locals {
  s3_origin_id = "MyS3Origin"
}
```
[Local Values](https://developer.hashicorp.com/terraform/language/values/locals)

<!-- TOC --><a name="terraform-data-sources"></a>
## Terraform Data Sources

This allows use to source data from cloud resources.

This is useful when we want to reference cloud resources without importing them.

```tf
data "aws_caller_identity" "current" {}

output "account_id" {
  value = data.aws_caller_identity.current.account_id
}
```
[Data Sources](https://developer.hashicorp.com/terraform/language/data-sources)

<!-- TOC --><a name="working-with-json"></a>
## Working with JSON

We use the jsonencode to create the json policy inline in the hcl.

```tf
> jsonencode({"hello"="world"})
{"hello":"world"}
```

[jsonencode](https://developer.hashicorp.com/terraform/language/functions/jsonencode)

<!-- TOC --><a name="aws-infra-ss"></a>
### AWS Infra SS
- CFN ![CFN](image-1.png)

- S3-bucket-objects ![S3-bucket-objects](image-2.png)

<!-- TOC --><a name="changine-the-lifecycle"></a>
### Changine the lifecycle 

[Meta Arguments Lifcycle](https://developer.hashicorp.com/terraform/language/meta-arguments/lifecycle)


<!-- TOC --><a name="terraform-data"></a>
## Terraform Data

Plain data values such as Local Values and Input Variables don't have any side-effects to plan against and so they aren't valid in replace_triggered_by. You can use terraform_data's behavior of planning an action each time input changes to indirectly use a plain value to trigger replacement.

https://developer.hashicorp.com/terraform/language/resources/terraform-data

<!-- TOC --><a name="provisioners"></a>
## Provisioners

Provisioners allow you to execute commands on compute instances eg. a AWS CLI command.

They are not recommended for use by Hashicorp because Configuration Management tools such as Ansible are a better fit, but the functionality exists.

[Provisioners](https://developer.hashicorp.com/terraform/language/resources/provisioners/syntax)

<!-- TOC --><a name="local-exec"></a>
### Local-exec

This will execute command on the machine running the terraform commands eg. plan apply

```tf
resource "aws_instance" "web" {
  # ...

  provisioner "local-exec" {
    command = "echo The server's IP address is ${self.private_ip}"
  }
}
```

https://developer.hashicorp.com/terraform/language/resources/provisioners/local-exec

<!-- TOC --><a name="remote-exec"></a>
### Remote-exec

This will execute commands on a machine which you target. You will need to provide credentials such as ssh to get into the machine.

```tf
resource "aws_instance" "web" {
  # ...

  # Establishes connection to be used by all
  # generic remote provisioners (i.e. file/remote-exec)
  connection {
    type     = "ssh"
    user     = "root"
    password = var.root_password
    host     = self.public_ip
  }

  provisioner "remote-exec" {
    inline = [
      "puppet apply",
      "consul join ${aws_instance.web.private_ip}",
    ]
  }
}
```
https://developer.hashicorp.com/terraform/language/resources/provisioners/remote-exec

<!-- TOC --><a name="for-each-expressions"></a>
## For Each Expressions

For each allows us to enumerate over complex data types

```sh
[for s in var.list : upper(s)]
```

This is mostly useful when you are creating multiples of a cloud resource and you want to reduce the amount of repetitive terraform code.

[For Each Expressions](https://developer.hashicorp.com/terraform/language/expressions/for)

<!-- TOC --><a name="terraform-console"></a>
### terraform console

```tf
 terraform console
> path.root
"."

> fileset("${path.root}/public/assets","*")
toset([
  "data.jpg",
  "thanos.png",
])

> fileset("${path.root}/public/assets", "*.{jpg,png,gif}")
toset([
  "data.jpg",
  "thanos.png",
])
```

<!-- TOC --><a name="week-1-expected-output-screenshots"></a>
### Week 1 - Expected output screenshots

![Alt text](image-3.png)

![Alt text](image-4.png)

        