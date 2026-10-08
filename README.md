# Pipeline-testing

A learning repo for **Azure** and **CI/CD pipelines** (YAML), using Terraform to provision Azure resources.

## What it does

Creates one or more Azure resource groups from a map defined in `terraform.tfvars`.

## Files

| File                  | Purpose                                                        |
|-----------------------|----------------------------------------------------------------|
| `provider.tf`         | Pins the `azurerm` provider (v4.70.0)                          |
| `main.tf`             | Declares the `rgs` variable and creates one resource group per entry using `for_each` |
| `terraform.tfvars`    | The resource groups to create                                  |
| `.terraform.lock.hcl` | Locks provider versions so every run uses the same build       |

## Prerequisites

- [Terraform](https://developer.hashicorp.com/terraform/install)
- [Azure CLI](https://learn.microsoft.com/cli/azure/install-azure-cli)
- An Azure subscription

## Run locally

```bash
az login
export ARM_SUBSCRIPTION_ID=$(az account show --query id -o tsv)

terraform init
terraform plan
terraform apply
```

Clean up when done, so you're not billed for resources you've stopped using:

```bash
terraform destroy
```

## Adding a resource group

Add an entry to the `rgs` map in `terraform.tfvars`, then run `terraform plan` and `terraform apply`:

```hcl
rgs = {
  rg_example = {
    name     = "<resource-group-name>"
    location = "<azure-region>"
  }
}
```

## Learning roadmap

- [x] Terraform basics: provider, variables, `for_each`
- [ ] Remote state in an Azure Storage Account
- [ ] Service principal / OIDC authentication for pipelines
- [ ] YAML pipeline: `init` → `validate` → `plan` → `apply`
- [ ] Manual approval before `apply`
- [ ] Separate environments (dev / prod)

## Notes

- `terraform.tfstate` and `.terraform/` are git-ignored. The state file contains subscription details and must never be committed.
- Never commit credentials or secrets. Use pipeline variables or secret stores instead.
