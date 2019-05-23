# AWS tagging and naming conventions

This outlines suitable naming conventions for tagging and naming AWS resources in order to manage them more efficiently, to clearly identify them and their context and adhere to AWS resource tagging best practices.

| Key name | Data type | Definition | Example |
| ------------- | ------------- | ------------- | ------------- |
| Name* | Dash delimited data based on a collection of tags | Name of resource  | See format below  |
| Terraform*   | Boolean  | If created by Terraform  | true or false  |

### Resource naming default pattern format

`Prefix-ServiceCode-ApplicationTypeCode-EnvironmentCode-ResourceDescription`

#### Prefix (optional)

Usually 2 characters to group resources based on service or application type. eg `wp` for WordPress

#### ServiceCode

Defines the TNA service or product. eg `disco` for Discovery

#### ApplicationTypeCode

Defines the application type. eg `netcore` for .NET core, `amp` for apache, mysql and PHP

#### EnvironmentCode

Defines the environment. eg `dev` for Development, `test` for Test, `live` for Live

#### ResourceDescription

Resource type and/or description. eg `sg-public-access` for security groups in the public subnet

