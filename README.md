# Introduction

This repository gives coding conventions for Terraform's HashiCorp Configuration Language (HCL). Terraform allows infrastructure to be described as code. As such, we should adhere to a style guide to ensure readable and high quality code.

# Syntax

- Strings are in double-quotes.

## Spacing

Use 2 spaces when defining resources except when defining inline policies or other inline resources.

```
resource "aws_iam_role" "iam_role" {
  name = "${var.resource_name}-role"
  assume_role_policy = <<EOF
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Action": "sts:AssumeRole",
      "Principal": {
        "Service": "ec2.amazonaws.com"
      },
      "Effect": "Allow",
      "Sid": ""
    }
  ]
}
EOF
}
```

## Resource Block Alignment

Parameter definitions in a resource block should be aligned. The `terraform fmt` command can do this for you.

```
provider "aws" {
  access_key = "${var.aws_access_key}"
  secret_key = "${var.aws_secret_key}"
  region     = "us-east-1"
}
```


## Comments

When commenting use two "//" and a space in front of the comment.

```
// CREATE ELK IAM ROLE 
...
```

# Naming

## File Names

Create a separate resource file for each type of AWS resource. Similar resources should be defined in the same file and named accordingly.

```
ami.tf
autoscaling_group.tf
cloudwatch.tf
iam.tf
launch_configuration.tf
providers.tf
s3.tf
security_groups.tf
sns.tf
sqs.tf
user_data.sh
variables.tf
```

## Parameter, Meta-parameter and Variable Naming

A hyphen 
## Parameter Naming

When defining the resource 
To avoid confusion and comlexity: 

- __Only use an underscore (`_`) when naming Terraform resource TYPES, NAMES, etc__
- __Only use a hyphen (`-`) when naming the created resources.__

```
resource "aws_security_group" "security_group" {
  name = "${var.resource_name}-security-group"
...
```

A resource's NAME should be the same as the TYPE minus the provider.

```
resource "aws_autoscaling_group" "autoscaling_group" {
...
```

If there are multiple resources of the same TYPE defined, add a minimalistic identifier to differentiate between the two resources. A blank line should sperate resource definitions contained in the same file.

```
// Create Data S3 Bucket
resource "aws_s3_bucket" "data_s3_bucket" {
  bucket = "${var.environment_name}-data-${var.aws_region}"
  acl    = "private"
  versioning {
    enabled = true
  }
}

// Create Images S3 Bucket
resource "aws_s3_bucket" "images_s3_bucket" {
  bucket = "${var.environment_name}-images-${var.aws_region}"
  acl    = "private"
}
```
