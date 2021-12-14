# magma-aws

### Prerequisite

#### $ 1000.00 monthly cost

download terraform:
```bash
wget https://releases.hashicorp.com/terraform/1.0.11/terraform_1.0.11_darwin_amd64.zip
unzip terraform_1.0.11_darwin_amd64.zip
sudo mv terraform /usr/local/bin/terraform
rm terraform_1.0.11_darwin_amd64.zip
```

https://aws.amazon.com/marketplace/seller-profile?id=a28de92c-8c49-49cd-8c6e-c2b3294a82c7

generate secrets:
```bash
export AWS_DOMAIN=magmaindia.org
mkdir -p certs && cd certs && \
  ../01-generate-secrets.sh $AWS_DOMAIN \
  && cd -
```
---

### Install

Setup Magma Infrastructure:
```bash
terraform init --upgrade
terraform apply -target=module.orc8r
```
> `-auto-approve`

set kubernetes config file:
```bash
export KUBECONFIG=$PWD/kubeconfig_orc8r
```

install boto3:
```bash
pip3 install boto3
```

Setup Magma Secrets:
```bash
terraform apply -target=module.orc8r-app.null_resource.orc8r_seed_secrets

# NOTE: if this isn't your first time applying the orc8r_seed_secrets resource, you'll need to first 
terraform taint module.orc8r-app.null_resource.orc8r_seed_secrets
terraform destroy -target module.orc8r-app.null_resource.orc8r_seed_secrets
```

Install Magma:
```bash
terraform apply
```
---

### Uninstall

delete everything:
```bash
export KUBECONFIG=$PWD/kubeconfig_orc8r
terraform destroy
# OR
terraform destroy -target=module.orc8r
```
---

### FAQ

Delete secret:
```bash
aws --region us-east-2 secretsmanager list-secrets
aws --region us-east-2 secretsmanager delete-secret --secret-id orc8r-secrets
```
> `--force-delete-without-recovery`

### OLD

#### Use for 1.5.0 Orc8r

Use Terraform v0.14.11
```bash
export TERRAFORM_VERSION=0.14.11
wget https://releases.hashicorp.com/terraform/${TERRAFORM_VERSION}/terraform_${TERRAFORM_VERSION}_linux_amd64.zip
unzip terraform_${TERRAFORM_VERSION}_linux_amd64.zip
sudo mv terraform /usr/bin/terraform
rm terraform_${TERRAFORM_VERSION}_linux_amd64.zip
```



