# AWS tagging and naming conventions

This outlines suitable naming conventions for tagging and naming AWS resources in order to manage them more efficiently, to clearly identify them and their context and adhere to AWS resource tagging best practices.

## Resource naming default pattern format

`Prefix-ServiceCode-ApplicationTypeCode-EnvironmentCode-ResourceDescription`

#### Prefix (optional)

Usually 2 or 3 characters to group resources based on service or application type. eg `wp` for WordPress

#### ServiceCode

Defines the TNA service or product. eg `disco` for Discovery

#### ApplicationTypeCode (optional, but mandatory for ec2 instances)

Defines the application type. eg `netcore` for .NET core, `amp` for Apache, MySQL and PHP

#### EnvironmentCode

Defines the environment. eg `dev` for Development, `test` for Test, `live` for Live

#### ResourceDescription

Resource type and/or description. eg `sg-public-access` for security groups in the public subnet

### Resource naming examples

`disco-netcore-live-api` - Discovery .NET core API ec2 instance on Live

`wp-blog-amp-test-efs-storage` - Blog WordPress EFS storage on Test

`win-jenkins-inter-sg-ip-access` - Windows Jenkins security group with defined IP access on Intersite

## Tagging formats

| Key name | Data type | Definition | Examples |
| ------------- | ------------- | ------------- | ------------- |
| Name* | Dash delimited data based on a collection of tags | Name of resource  | [See format above](#resource-naming-default-pattern-format)  |
| tna:Service*   | String  | Predifined set of organisation services or products   | disco, blog or cdn  |
| tna:ApplicationType*   | String  | The application type   | netcore, amp or nodejs  |
| tna:Environment*   | String  | Predifined set of environments   | dev, test or live  |
| tna:CostCentre*   | integer | Cost centre code   | dev, test or live  |
| Terraform*   | Boolean  | Terraform created   | true or false  |




