helm lint ./my-helm-chart

helm template my-release ./my-helm-chart

helm install my-release ./my-helm-chart --dry-run --debug

helm upgrade my-release ./my-helm-chart --dry-run --debug

oc auth can-i create deployment --namespace=my-namespace

helm install my-release ./my-helm-chart --namespace my-namespace

helm upgrade my-release ./my-helm-chart --namespace my-namespace

helm list --namespace my-namespace

oc get all -n my-namespace

oc get pods -n my-namespace
oc describe pod <pod-name> -n my-namespace

helm history my-release --namespace my-namespace

helm rollback my-release [REVISION] --namespace my-namespace

helm uninstall my-release --namespace my-namespace
