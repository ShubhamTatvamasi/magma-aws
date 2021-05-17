# magma-aws

clone magma 1.5
```bash
git clone -b v1.5 https://github.com/magma/magma.git
```

generate secrets:
```bash
mkdir -p certs && cd certs && \
  ../01-generate-secrets.sh orc8r.rxdp.in \
  && cd -
```
---

install terraform:
```bash
terraform init
terraform plan
```
