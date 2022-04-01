# magma-aws

### Prerequisite

#### $ 1,000.00 monthly cost

download terraform:
```bash
wget https://releases.hashicorp.com/terraform/1.0.11/terraform_1.0.11_darwin_amd64.zip
unzip terraform_1.0.11_darwin_amd64.zip
rm terraform_1.0.11_darwin_amd64.zip
sudo mv terraform /usr/local/bin/terraform
```

https://aws.amazon.com/marketplace/seller-profile?id=a28de92c-8c49-49cd-8c6e-c2b3294a82c7

generate secrets:
```bash
export AWS_DOMAIN=orc8r.magmacore.link
mkdir -p certs && cd certs && \
  ../01-generate-secrets.sh $AWS_DOMAIN \
  && cd -
```
---

### Install

Install boto3 package::
```bash
pip3 install boto3
```

Init terraform:
```bash
terraform init --upgrade
```

Setup Magma Secrets:
```bash
terraform apply -target=module.orc8r-app.null_resource.orc8r_seed_secrets -auto-approve

# NOTE: if this isn't your first time applying the orc8r_seed_secrets resource, you'll need to first 
terraform taint module.orc8r-app.null_resource.orc8r_seed_secrets
terraform destroy -target module.orc8r-app.null_resource.orc8r_seed_secrets
```

Setup Magma Infrastructure:
```bash
terraform apply -target=module.orc8r -auto-approve
```

Install Magma:
```bash
terraform apply
```

set kubernetes config file:
```bash
cd ~/.kube
touch orc8r.magmacore.link
export KUBECONFIG=${PWD}/orc8r.magmacore.link

aws eks update-kubeconfig \
  --name orc8r \
  --region us-east-2
```
---

### Uninstall

delete everything:
```bash
terraform destroy -target=module.orc8r-app -auto-approve
terraform destroy -auto-approve
terraform destroy -target=module.orc8r.aws_route53_zone.orc8r -auto-approve
```
---

### FAQ

Delete secret:
```bash
aws secretsmanager list-secrets \
  --region us-east-2

aws secretsmanager delete-secret \
  --secret-id orc8r-secrets \
  --force-delete-without-recovery \
  --region us-east-2
```

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
