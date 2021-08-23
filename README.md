# magma-aws

### Prerequisite

#### $ 900.00 monthly cost

Use Terraform v0.14.11
```bash
export TERRAFORM_VERSION=0.14.11
wget https://releases.hashicorp.com/terraform/${TERRAFORM_VERSION}/terraform_${TERRAFORM_VERSION}_linux_amd64.zip
unzip terraform_${TERRAFORM_VERSION}_linux_amd64.zip
sudo mv terraform /usr/bin/terraform
rm terraform_${TERRAFORM_VERSION}_linux_amd64.zip
```

generate secrets:
```bash
AWS_DOMAIN=magmalocal.com
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
```


