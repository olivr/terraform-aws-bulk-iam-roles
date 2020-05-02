# terraform-aws-assumable-roles

## Examples

```hcl
module "roles" {
  source = "github.com/OlivrDotCom/terraform-aws-assumable-roles"

  roles = {

    CrossAccountAdminRole = {
      policies               = ["arn:aws:iam::aws:policy/AdministratorAccess"]
      assumable_by_roles     = [
        "arn:aws:iam::111111111111:root",
        "arn:aws:iam::111111111111:role/AdministratorRole"]
    }

    ViewOnlyFederatedRole = {
      policies               = [
        "arn:aws:iam::aws:policy/job-function/ViewOnlyAccess",
        "arn:aws:iam::aws:policy/ReadOnlyAccess"
      ]
      assumable_by_federated = ["arn:aws:iam::111111111111:saml-provider/my-saml"]
    }

    NoAccessRole = {
      assumable_by_federated = ["arn:aws:iam::111111111111:saml-provider/my-saml"]
    }

  }
}

output "roles_arn" {
  value = module.roles.arn
}

output "noaccessrole_arn" {
  value = module.roles.arn["NoAccessRole"]
}
```

## Requirements

| Name      | Version    |
| --------- | ---------- |
| terraform | ~> 0.12.24 |
| aws       | ~> 2.58    |

## Providers

| Name | Version |
| ---- | ------- |
| aws  | ~> 2.58 |

## Inputs

| Name  | Description     | Type                     | Default | Required |
| ----- | --------------- | ------------------------ | ------- | :------: |
| roles | Roles to create | `map(map(list(string)))` | n/a     |   yes    |

## Outputs

| Name | Description                        |
| ---- | ---------------------------------- |
| arn  | ARNs for each of the created roles |
