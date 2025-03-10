helm repo add nats https://nats-io.github.io/k8s/helm/charts/
helm repo update

helm install nats nats/nats --namespace nats --create-namespace

kubectl get pods -n nats
kubectl get svc -n nats