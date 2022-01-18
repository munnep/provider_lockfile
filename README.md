# Provider Lockfile

In this repository you will take a look at the provider lockfile introduced in Terraform 0.14

Information about the Terraform lockfile [See documentation](https://www.hashicorp.com/blog/terraform-0-14-introduces-a-dependency-lock-file-for-providers)

You will download an older AWS provider version. Change the version and run a Terraform initialization again and see what happens

# Prerequisites

## Install terraform  
See the following documentation [How to install Terraform](https://learn.hashicorp.com/tutorials/terraform/install-cli)

# How to

- Clone the repository to your local machine
```
git clone https://github.com/munnep/provider_lockfile.git
```
- Change your directory
```
cd provider_lockfile
```
- Terraform initialize
```
terraform init
```
- See the created file ```.terraform.lock.hcl``` for the version downloaded
```
...
...
...
# This file is maintained automatically by "terraform init".
# Manual edits may be lost in future updates.

provider "registry.terraform.io/hashicorp/aws" {
  version     = "3.27.0"
  constraints = "3.27.0"
...
...
...
```
- change the value from ```3.27``` to ```3.72``` in the file ```provider.tf```
```
terraform {
  required_providers {
    aws = {
      source  = "hashicorp/aws"
      version = "3.72"
    }
  }
}
```
- Terraform initialize
```
terraform init
```
- You will get an error stating this doesn't match the lockfile. 
```
╷
│ Error: Failed to query available provider packages
│ 
│ Could not retrieve the list of available versions for provider hashicorp/aws: locked provider
│ registry.terraform.io/hashicorp/aws 3.27.0 does not match configured version constraint 3.72.0; must
│ use terraform init -upgrade to allow selection of new versions
```
- Terraform initialize again with the upgrade parameter
```
terraform init -upgrade
```
```
...
...
...
Initializing provider plugins...
- Finding hashicorp/aws versions matching "3.72.0"...
- Installing hashicorp/aws v3.72.0...
- Installed hashicorp/aws v3.72.0 (signed by HashiCorp)

Terraform has made some changes to the provider dependency selections recorded
in the .terraform.lock.hcl file. Review those changes and commit them to your
version control system if they represent changes you intended to make.

Terraform has been successfully initialized!
...
...
...
```