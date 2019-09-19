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

#### Role (optional, but mandatory for ec2 instances, load balancers, security groups and RDS)

Predifined roles. eg `web` or `pub` for resources in a public subnet, `api` or `prvt` for an application ec2 in a private subnet, `ma` for a master RDS instance `rr` for a read replica RDS instance

### Resource naming examples

#### ec2 instance
`disco-netcore-live-ec2-api` (Discovery .NET core API ec2 instance on Live)

#### EFS
`wp-blog-amp-test-efs-storage` (Blog WordPress (Apache, MySQL and PHP) EFS storage on Test)

#### Security group
`win-jenkins-inter-sg-ip-access` (Windows Jenkins security group with defined IP access on Intersite)

#### IAM role
`disco-web-test-ec2-role` (IAM role assumed by Discovery web app ec2 instance on Test)

#### IAM ploicy
`disco-downloads-s3-access-policy` (IAM policy for accessing the downloads s3 bucket for Discovery)

## Tagging formats

| Key name | Data type | Definition | Examples |
| ------------- | ------------- | ------------- | ------------- |
| Name* | String | Name of resource  | [See format above](#resource-naming-default-pattern-format)  |
| Service*   | String  | Predifined set of organisation services or products   | disco, blog or cdn  |
| ApplicationType*   | String  | The application type   | netcore, amp or nodejs  |
| Role*   | String  | Predifined roles   | pub, prvt, web, api or db  |
| Environment*   | String  | Predifined set of environments   | dev, test or live  |
| CostCentre*   | integer | Cost centre code   | 53  |
| Owner   | String  | email or team name  | automate and modernise  |
| CreatedBy   | String  | email   | auto.modernise@nationalarchives.gov.uk  |
| Terraform*   | Boolean  | Terraform created   | true or false  |

## Tag style rules

* Tag key names should use Pascal case. eg, ApplicationType
* Tag values should use lower case with dashes. eg, disco-netcore-live-ec2-api
* Tag values are case-sensitive and should not use the semi-colon (";"), equal sign ("="), or pipe ("|") characters as these are used as delimiters in compound values.
* Compound tag value key names should use Pascal case followed by an equal sign ("=") such as KeyName1=value1|value2|value3;KeyName2=value1|value2|value3

## Terraform

Terraform example
```bash
# EC2 instance
resource "aws_instance" "api" {
  ami           = "${data.aws_ami.ubuntu.id}"
  instance_type = "t2.micro"

  tags = {
    Name            = "commandpapers-netcore-test-ec2-api"
    Service         = "commandpapers"
    ApplicationType = "netcore"
    Role            = "api"
    Environment     = "test"
    CostCentre      = 00
    Owner           = "auto-modernise"
    CreatedBy       = "auto.modernise@nationalarchives.gov.uk"
    Terraform       = true
  }
}

# RDS instance
resource "aws_db_instance" "master" {
  name                 = "commandpapers-mysql-test-db-ma"
  identifier           = "commandpapers-mysql-test-db-ma"
  allocated_storage    = 20
  storage_type         = "gp2"
  engine               = "mysql"
  engine_version       = "5.7"
  instance_class       = "db.t2.micro"
  username             = "foo"
  password             = "foobarbaz"
  parameter_group_name = "default.mysql5.7"
  
  tags = {
    Service         = "commandpapers"
    ApplicationType = "mysql"
    Role            = "ma"
    Environment     = "test"
    CostCentre      = 00
    Owner           = "auto-modernise"
    CreatedBy       = "auto.modernise@nationalarchives.gov.uk"
    Terraform       = true
  }
}

# Security group
resource "aws_security_group" "public_access" {
  name = "commandpapers-netcore-test-sg-pub"
  description = "HTTP and HTTPS public access"
  vpc_id = "${data.terraform_remote_state.env_vpc.vpc}"

  tags = {
    Service         = "commandpapers"
    ApplicationType = "netcore"
    Role            = "pub"
    Environment     = "test"
    CostCentre      = 00
    Owner           = "auto-modernise"
    CreatedBy       = "auto.modernise@nationalarchives.gov.uk"
    Terraform       = true
  }
}

# IAM role
resource "aws_iam_role" "commandpapers_ec2" {
  name               = "commandpapers-web-test-ec2-role"
  
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

# IAM policy
resource "aws_iam_policy" "commandpapers_s3_access" {
  name        = "commandpapers-downloads-s3-access-policy"
  description = "Downloads s3 access"
  role = "${aws_iam_role.commandpapers_s3_access.id}"

  policy = <<EOF
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "s3:PutObject",
        "s3:GetObject"
      ],
      "Resource": [
         "arn:aws:s3:::downloads-s3-bucket/*"
      ]
    }
  ]
}
EOF
}
```

Terraform variable example
```bash
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
  default = 00
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
    Role = "web"
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
    Role = "api"
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
