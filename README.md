# AWS tagging and naming conventions

This outlines suitable naming conventions for tagging and naming AWS resources in order to manage them more efficiently, to clearly identify them and their context and adhere to AWS resource tagging best practices.

## Resource naming default pattern format

Dash delimited data based on a collection of tags

`Prefix-ServiceCode-ApplicationTypeCode-EnvironmentCode-Resource-Role`

#### Prefix (optional)

Usually 2 or 3 characters to group resources based on service or application type. eg `wp` for WordPress

#### ServiceCode

Defines the TNA service or product. eg `disco` for Discovery

#### ApplicationTypeCode (optional, but mandatory for ec2 instances)

Defines the application type. eg `netcore` for .NET core, `amp` for Apache, MySQL and PHP

#### EnvironmentCode

Defines the environment. eg `dev` for Development, `test` for Test, `live` for Live

#### Resource

Resource type and/or description. eg `sg` for security groups, `lb` for application load balancer

#### Role (optional, but mandatory for ec2 instances, load balancers and RDS)

Predifined roles. eg `web` or `pub` for resources in a public subnet, `api` or `prvt` for an application ec2 in a private subnet, `db` for a RDS instance

### Resource naming examples

`disco-netcore-live-ec2-api` (Discovery .NET core API ec2 instance on Live)

`wp-blog-amp-test-efs-storage` (Blog WordPress (Apache, MySQL and PHP) EFS storage on Test)

`win-jenkins-inter-sg-ip-access` (Windows Jenkins security group with defined IP access on Intersite)

## Tagging formats

| Key name | Data type | Definition | Examples |
| ------------- | ------------- | ------------- | ------------- |
| Name* | String | Name of resource  | [See format above](#resource-naming-default-pattern-format)  |
| Service*   | String  | Predifined set of organisation services or products   | disco, blog or cdn  |
| ApplicationType*   | String  | The application type   | netcore, amp or nodejs  |
| ApplicationRole*   | String  | Predifined roles   | pub, prvt, web, api or db  |
| Environment*   | String  | Predifined set of environments   | dev, test or live  |
| CostCentre*   | integer | Cost centre code   | 63  |
| Owner   | String  | email or team name  | Automate and modernise  |
| CreatedBy   | String  | email   | auto.modernise@nationalarchives.gov.uk  |
| Terraform*   | Boolean  | Terraform created   | true or false  |

## Tag style rules

* Tag key names should use Pascal case. eg, ApplicationType
* Tag values should use lower case with dashes. eg, disco-netcore-live-ec2-api
* Tag values are case-sensitive and should not use the semi-colon (";"), equal sign ("="), or pipe ("|") characters as these are used as delimiters in compound values.
* Compound tag value key names should use CamelCase followed by an equal sign ("=") such as KeyName1=value1|value2|value3;KeyName2=value1|value2|value3

## Terraform

Terraform example
```
resource "aws_instance" "api" {
  ami           = "${data.aws_ami.ubuntu.id}"
  instance_type = "t2.micro"

  tags = {
    Name = "commandpapers-netcore-test-ec2-api"
    Service = "commandpapers"
    ApplicationType = "netcore"
    ApplicationRole = "api"
    Environment = "test"
    CostCentre = 63
    Owner = "auto-modernise"
    CreatedBy = "auto.modernise@nationalarchives.gov.uk"
    Terraform = true
  }
}
```

Terraform variable example
```
variable "service" {
  default = "commandpapers"
}

variable "app" {
  default = "netcore"
}

variable "env" {
  default = "test"
}

variable "costcentre" {
  default = 63
}

variable "owner" {
  default = "auto-modernise"
}

variable "createdby" {
  default = "auto.modernise@nationalarchives.gov.uk"
}

resource "aws_instance" "web" {
  ami           = "${data.aws_ami.ubuntu.id}"
  instance_type = "t2.micro"

  tags = {
    Name = "${var.service}-${var.app}-${var.env}-ec2-web"
    Service = "${var.service}"
    ApplicationType = "${var.app}"
    ApplicationRole = "web"
    Environment = "${var.env}"
    CostCentre = ${var.costcentre}
    Owner = "${var.owner}"
    CreatedBy = "${var.createdby}"
    Terraform = true
  }
}

resource "aws_instance" "api" {
  ami           = "${data.aws_ami.ubuntu.id}"
  instance_type = "t2.micro"

  tags = {
    Name = "${var.service}-${var.app}-${var.env}-ec2-api"
    Service = "${var.service}"
    ApplicationType = "${var.app}"
    ApplicationRole = "api"
    Environment = "${var.env}"
    CostCentre = ${var.costcentre}
    Owner = "${var.owner}"
    CreatedBy = "${var.createdby}"
    Terraform = true
  }
}
```

## References

* [AWS Tagging Strategies](https://aws.amazon.com/answers/account-management/aws-tagging-strategies/)
* [A guide to tagging resources in AWS](https://medium.com/stax-blog/a-guide-to-tagging-resources-in-aws-8f4311afeb46)
