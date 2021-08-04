# get-aws-certs

```bash
kubectl get secrets nms-certs \
  -o jsonpath='{.data.admin_operator\.key\.pem}' | base64 -d > admin_operator.key.pem

kubectl get secrets fluentd-certs \
  -o jsonpath='{.data.fluentd\.key}' | base64 -d > fluentd.key

kubectl get secrets fluentd-certs \
  -o jsonpath='{.data.fluentd\.pem}' | base64 -d > fluentd.pem

kubectl get secrets orc8r-certs \
  -o jsonpath='{.data.admin_operator\.pem}' | base64 -d > admin_operator.pem

kubectl get secrets orc8r-certs \
  -o jsonpath='{.data.bootstrapper\.key}' | base64 -d > bootstrapper.key

kubectl get secrets orc8r-certs \
  -o jsonpath='{.data.certifier\.key}' | base64 -d > certifier.key

kubectl get secrets orc8r-certs \
  -o jsonpath='{.data.certifier\.pem}' | base64 -d > certifier.pem

kubectl get secrets orc8r-certs \
  -o jsonpath='{.data.controller\.crt}' | base64 -d > controller.crt

kubectl get secrets orc8r-certs \
  -o jsonpath='{.data.controller\.key}' | base64 -d > controller.key

kubectl get secrets orc8r-certs \
  -o jsonpath='{.data.rootCA\.pem}' | base64 -d > rootCA.pem
```
---

backup secrets:
```bash
kubectl get secrets nms-certs -o yaml > nms-certs.yaml
kubectl get secrets fluentd-certs -o yaml > fluentd-certs.yaml
kubectl get secrets orc8r-certs -o yaml > orc8r-certs.yaml
```

restore secrets:
```bash
kubectl apply -f nms-certs.yaml
kubectl apply -f fluentd-certs.yaml
kubectl apply -f orc8r-certs.yaml
```
---

### Create certs

create Orc8r secrets from files: 
```bash
kubectl create secret generic fluentd-certs \
  --from-file=certifier.pem \
  --from-file=fluentd.pem \
  --from-file=fluentd.key

kubectl create secret generic nms-certs \
  --from-file=admin_operator.key.pem \
  --from-file=admin_operator.pem \
  --from-file=controller.crt \
  --from-file=controller.key

kubectl create secret generic orc8r-certs \
  --from-file=admin_operator.pem \
  --from-file=bootstrapper.key \
  --from-file=certifier.key \
  --from-file=certifier.pem \
  --from-file=controller.crt \
  --from-file=controller.key \
  --from-file=rootCA.pem
```

