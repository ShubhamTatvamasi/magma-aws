# magma-aws

Use Terraform v0.14.11

generate secrets:
```bash
mkdir -p certs && cd certs && \
  ../01-generate-secrets.sh magmalocal.com \
  && cd -
```
---

Setup Magma Infrastructure:
```bash
terraform init
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
```

Install Magma:
```bash
terraform apply
```
