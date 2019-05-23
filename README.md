# AWS tagging and naming conventions

This outlines suitable naming conventions for tagging and naming AWS resources in order to manage them more efficiently, to clearly identify them and their context and adhere to AWS resource tagging best practices.

## Resource naming default pattern format

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

#### Role (optional, but mandatory for ec2 instances)

Predifined roles. eg `web` or `pub` for resources in a public subnet, `api` or `prvt` for an application ec2 in a private subnet, `db` for a RDS instance

### Resource naming examples

`disco-netcore-live-ec2-api` (Discovery .NET core API ec2 instance on Live)

`wp-blog-amp-test-efs-storage` (Blog WordPress (Apache, MySQL and PHP) EFS storage on Test)

`win-jenkins-inter-sg-ip-access` (Windows Jenkins security group with defined IP access on Intersite)

## Tagging formats

| Key name | Data type | Definition | Examples |
| ------------- | ------------- | ------------- | ------------- |
| Name* | Dash delimited data based on a collection of tags | Name of resource  | [See format above](#resource-naming-default-pattern-format)  |
| tna:Service*   | String  | Predifined set of organisation services or products   | disco, blog or cdn  |
| tna:ApplicationType*   | String  | The application type   | netcore, amp or nodejs  |
| tna:Role*   | String  | Predifined roles   | public, private, web, api or db  |
| tna:Environment*   | String  | Predifined set of environments   | dev, test or live  |
| tna:CostCentre*   | integer | Cost centre code   | 63  |
| tna:Owner   | String  | email or team name  | Automate and modernise  |
| tna:CreatedBy   | String  | email   | auto.modernise@nationalarchives.gov.uk  |
| Terraform*   | Boolean  | Terraform created   | true or false  |




