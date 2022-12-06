# pre-commit-hook
The pre-commit hook is run first before you even type in a commit message. It's used to inspect the snapshot that's about to be committed, to see if you've forgotten something, to make sure tests run, or to examine whatever you need to inspect in the code.

Features

Using git pre-commit hooks in your software delivery pipeline helps achieve better quality software with faster time to market

Developers have more access to define additional aspects around the infrastructure needed to run their applications. It is critical to lower the risk of changes completed in development to supply additional confidence and increase velocity.

Implementing git pre-commit hooks is essentially executing local scripts to check for certain quality and policy items prior to pushing the commit

# Prerequisites:
terraform

terraform installtion Ubuntu/Debian


sudo apt-get update && sudo apt-get install -y gnupg software-properties-common
 

```
wget -O- https://apt.releases.hashicorp.com/gpg | \
    gpg --dearmor | \
    sudo tee /usr/share/keyrings/hashicorp-archive-keyring.gpg
 


gpg --no-default-keyring \
    --keyring /usr/share/keyrings/hashicorp-archive-keyring.gpg \
    --fingerprint
 


echo "deb [signed-by=/usr/share/keyrings/hashicorp-archive-keyring.gpg] \
    https://apt.releases.hashicorp.com $(lsb_release -cs) main" | \
    sudo tee /etc/apt/sources.list.d/hashicorp.list
```



 


sudo apt update
 


sudo apt-get install terraform
verify the installation


terraform -help
 

Terraform installation macos


brew tap hashicorp/tap

brew install hashicorp/tap/terraform

brew update

brew upgrade hashicorp/tap/terraform
Verify the installation


terraform -help
 

install git

Install dependencies

```
pre-commit, terraform, git, POSIX compatible shell, Internet connection (on first run), x86_64 compatible operation system, Some hardware where this OS will run, Electricity for hardware and internet connection, Some basic physical laws, Hope that it all will work.
checkov required for checkov hook.
terraform-docs required for terraform_docs hook.
terragrunt required for terragrunt_validate hook.
terrascan required for terrascan hook.
TFLint required for terraform_tflint hook.
TFSec required for terraform_tfsec hook.
infracost required for infracost_breakdown hook.
jq required for infracost_breakdown hook.
tfupdate required for tfupdate hook.
hcledit required for terraform_wrapper_module_for_each hook.
```

# Install on macOS

```
brew install pre-commit terraform-docs tflint tfsec checkov terrascan infracost tfupdate minamijoyo/hcledit/hcledit jq
```

# Install on Ubuntu 20.04

```
sudo apt update
sudo apt install -y unzip software-properties-common python3 python3-pip
python3 -m pip install --upgrade pip
pip3 install --no-cache-dir pre-commit
pip3 install --no-cache-dir checkov
curl -L "$(curl -s https://api.github.com/repos/terraform-docs/terraform-docs/releases/latest | grep -o -E -m 1 "https://.+?-linux-amd64.tar.gz")" > terraform-docs.tgz && tar -xzf terraform-docs.tgz terraform-docs && rm terraform-docs.tgz && chmod +x terraform-docs && sudo mv terraform-docs /usr/bin/
curl -L "$(curl -s https://api.github.com/repos/tenable/terrascan/releases/latest | grep -o -E -m 1 "https://.+?_Linux_x86_64.tar.gz")" > terrascan.tar.gz && tar -xzf terrascan.tar.gz terrascan && rm terrascan.tar.gz && sudo mv terrascan /usr/bin/ && terrascan init
curl -L "$(curl -s https://api.github.com/repos/terraform-linters/tflint/releases/latest | grep -o -E -m 1 "https://.+?_linux_amd64.zip")" > tflint.zip && unzip tflint.zip && rm tflint.zip && sudo mv tflint /usr/bin/
curl -L "$(curl -s https://api.github.com/repos/aquasecurity/tfsec/releases/latest | grep -o -E -m 1 "https://.+?tfsec-linux-amd64")" > tfsec && chmod +x tfsec && sudo mv tfsec /usr/bin/
sudo apt install -y jq && \
curl -L "$(curl -s https://api.github.com/repos/infracost/infracost/releases/latest | grep -o -E -m 1 "https://.+?-linux-amd64.tar.gz")" > infracost.tgz && tar -xzf infracost.tgz && rm infracost.tgz && sudo mv infracost-linux-amd64 /usr/bin/infracost && infracost register
curl -L "$(curl -s https://api.github.com/repos/minamijoyo/tfupdate/releases/latest | grep -o -E -m 1 "https://.+?_linux_amd64.tar.gz")" > tfupdate.tar.gz && tar -xzf tfupdate.tar.gz tfupdate && rm tfupdate.tar.gz && sudo mv tfupdate /usr/bin/
curl -L "$(curl -s https://api.github.com/repos/minamijoyo/hcledit/releases/latest | grep -o -E -m 1 "https://.+?_linux_amd64.tar.gz")" > hcledit.tar.gz && tar -xzf hcledit.tar.gz hcledit && rm hcledit.tar.gz && sudo mv hcledit /usr/bin/
``` 

# Initialize git
You need to initialize git on the working directory before applying any pre-commit hook


git init
initialized Git repository in /Users/firojmohammad/terraform/terraform-ubuntu/.git/
Install pre-commit before git commit



pre-commit install
pre-commit installed at .git/hooks/pre-commit
you can check at .git/hooks/pre-commit 

Here are all your hooks 


# cat .git/hooks/pre-commit
 ```
#!/usr/bin/env bash
# File generated by pre-commit: https://pre-commit.com
# ID: 138fd403232d2ddd5efb44317e38bf03

# start templated
INSTALL_PYTHON=/opt/homebrew/opt/pre-commit/libexec/bin/python3
ARGS=(hook-impl --config=.pre-commit-config.yaml --hook-type=pre-commit)
# end templated

HERE="$(cd "$(dirname "$0")" && pwd)"
ARGS+=(--hook-dir "$HERE" -- "$@")

if [ -x "$INSTALL_PYTHON" ]; then
    exec "$INSTALL_PYTHON" -mpre_commit "${ARGS[@]}"
elif command -v pre-commit > /dev/null; then
    exec pre-commit "${ARGS[@]}"
else
    echo '`pre-commit` not found.  Did you forget to activate your virtualenv?' 1>&2
    exit 1
fi
```
 

# Create config file .pre-commit-config.yaml
You can write a pre-commit hook config file according to the depth of the code, script language, etc. This one is for the terraform code.


- repo: https://github.com/antonbabenko/pre-commit-terraform
  rev: v1.75.0 # Get the latest from: https://github.com/gruntwork-io/pre-commit/releases
  hooks:
    - id: terraform_tflint
    - id: terraform_fmt
    - id: terraform_tfsec
    - id: terraform_docs
    - id: terraform_validate
- repo: https://github.com/pre-commit/pre-commit-hooks
  rev: v2.1.0
  hooks:
    - id: check-merge-conflict
    - id: check-yaml
    - id: check-added-large-files
    - id: trailing-whitespace
 

Then we verify the code and it must be as shown in 

```
ls -la
total 56
drwxr-xr-x  11 firojmohammad  staff   352 Nov 23 16:59 .
drwxr-xr-x   5 firojmohammad  staff   160 Sep 26 15:32 ..
drwxr-xr-x   7 firojmohammad  staff   224 Nov 23 16:59 .git
-rw-r--r--   1 firojmohammad  staff   492 Nov 23 13:16 .pre-commit-config.yaml
drwxr-xr-x   4 firojmohammad  staff   128 Nov 21 11:14 .terraform
-rw-r--r--   1 firojmohammad  staff  1239 Nov 21 16:43 .terraform.lock.hcl
-rw-r--r--   1 firojmohammad  staff   138 Sep 26 15:32 backend.tf
-rw-r--r--   1 firojmohammad  staff   569 Nov 23 14:38 main.tf
-rw-r--r--   1 firojmohammad  staff   629 Sep 26 15:32 output.tf
-rw-r--r--   1 firojmohammad  staff    92 Nov 21 16:43 provider.tf
-rw-r--r--   1 firojmohammad  staff  1023 Nov 21 16:43 variable.tf
```
Whenever you change anything in your terraform code just do git add and git commit but whenever you do git commit the pre-commit hook must scan all the code in your repo

```
git add .
git commit -m "added something"
```
after that, you can check it by running the command git commit -m "added something" just after git add .

```
git commit -m "added something"
```

```
Terraform validate with tflint...........................................Failed
- hook id: terraform_tflint
- exit code: 2

\e[0m\e[32mCommand 'tflint --init' successfully done:\e[0m




\e[0m\e[33mTFLint in ./:\e[0m
1 issue(s) found:

Warning: Missing version constraint for provider "aws" in "required_providers" (terraform_required_providers)

  on provider.tf line 1:
   1: provider "aws" {

Reference: https://github.com/terraform-linters/tflint-ruleset-terraform/blob/v0.2.1/docs/rules/terraform_required_providers.md

Terraform fmt............................................................Passed
Terraform validate with tfsec............................................Failed
- hook id: terraform_tfsec
- exit code: 1

Results #1-3 HIGH Subnet associates public IP address. (3 similar results)
────────────────────────────────────────────────────────────────────────────────
  ../../modules/aws-vpc/main.tf:359
   via main.tf:2-33 (module.vpc)
────────────────────────────────────────────────────────────────────────────────
  352    resource "aws_subnet" "public" {
  ...  
  359  [   map_public_ip_on_launch         = var.map_public_ip_on_launch (true)
  ...  
  380    }
────────────────────────────────────────────────────────────────────────────────
  Individual Causes
  - ../../modules/aws-vpc/main.tf:2-33 (module.vpc)
  - ../../modules/aws-vpc/main.tf:2-33 (module.vpc) 2 instances
────────────────────────────────────────────────────────────────────────────────
          ID aws-ec2-no-public-ip-subnet
      Impact The instance is publicly accessible
  Resolution Set the instance to not be publicly accessible

  More Information
  - https://aquasecurity.github.io/tfsec/v1.28.1/checks/aws/ec2/no-public-ip-subnet/
  - https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/subnet#map_public_ip_on_launch
────────────────────────────────────────────────────────────────────────────────


Result #4 MEDIUM VPC Flow Logs is not enabled for VPC  
────────────────────────────────────────────────────────────────────────────────
  ../../modules/aws-vpc/main.tf:20-36
   via main.tf:2-33 (module.vpc)
────────────────────────────────────────────────────────────────────────────────
   20  ┌ resource "aws_vpc" "this" {
   21  │   count = local.create_vpc ? 1 : 0
   22  │ 
   23  │   cidr_block           = var.cidr
   24  │   instance_tenancy     = var.instance_tenancy
   25  │   enable_dns_hostnames = var.enable_dns_hostnames
   26  │   enable_dns_support   = var.enable_dns_support
   27  │   #enable_classiclink               = var.enable_classiclink
   28  └   #enable_classiclink_dns_support   = var.enable_classiclink_dns_support
   ..  
────────────────────────────────────────────────────────────────────────────────
          ID aws-ec2-require-vpc-flow-logs-for-all-vpcs
      Impact Without VPC flow logs, you risk not having enough information about network traffic flow to investigate incidents or identify security issues.
  Resolution Enable flow logs for VPC

  More Information
  - https://aquasecurity.github.io/tfsec/v1.28.1/checks/aws/ec2/require-vpc-flow-logs-for-all-vpcs/
────────────────────────────────────────────────────────────────────────────────


  timings
  ──────────────────────────────────────────
  disk i/o             1.413292ms
  parsing              116.586918ms
  adaptation           2.199875ms
  checks               4.98675ms
  total                125.186835ms

  counts
  ──────────────────────────────────────────
  modules downloaded   0
  modules processed    2
  blocks processed     377
  files read           10

  results
  ──────────────────────────────────────────
  passed               5
  ignored              0
  critical             0
  high                 3
  medium               1
  low                  0

  5 passed, 4 potential problem(s) detected.



Terraform docs...........................................................Passed
Terraform validate.......................................................Passed
Check for merge conflicts................................................Passed
Check Yaml...............................................................Passed
Trim Trailing Whitespace.................................................Passed
```
