# rollback

backup kubeconfig:
```bash
cp kubeconfig_orc8r kubeconfig_orc8r.bak
```

Recover 1.3.3 terraform `main.tf` file:
```bash
cp main.tf.bak main.tf
```

Update your dependencies:
```bash
terraform init --upgrade
terraform refresh
```

apply changes:
```bash
terraform apply -target=module.orc8r
```

