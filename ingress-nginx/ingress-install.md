## Ingres Nginx o'rnatish
```yaml
helm repo add ingress-nginx https://kubernetes.github.io/ingress-nginx
helm repo update
helm install ingress-nginx ingress-nginx/ingress-nginx --namespace ingress-nginx --create-namespace
```

```yaml
kubectl get pods -n ingress-nginx
```
```
output
NAME                                        READY   STATUS    RESTARTS   AGE
ingress-nginx-controller-7657f6db5f-p7669   1/1     Running   0          11s
```

```yaml
kubectl get svc -n ingress-nginx
```
```
output
NAME                                 TYPE           CLUSTER-IP       EXTERNAL-IP   PORT(S)                      AGE
ingress-nginx-controller             LoadBalancer   10.106.89.227    localhost     80:30819/TCP,443:30507/TCP   17s
ingress-nginx-controller-admission   ClusterIP      10.102.145.241   <none>        443/TCP                      17s
```
