```yaml
# values.yaml
harborAdminPassword: "YourSecurePassword123"  # Replace with your secure password

expose:
  type: ingress
  tls:
    enabled: true
    certSource: auto  # Uses self-signed certs for local dev
  ingress:
    hosts:
      core: harbor.local
    annotations:
      kubernetes.io/ingress.class: nginx

database:
  type: internal

redis:
  type: internal

persistence:
  enabled: true
  persistentVolumeClaim:
    registry:
      size: 10Gi
    chartmuseum:
      size: 5Gi
    jobservice:
      size: 1Gi
    database:
      size: 1Gi
    redis:
      size: 1Gi

registry:
  resources:
    requests:
      memory: 256Mi
      cpu: 100m

enableContentTrust: false

updateStrategy:
  type: RollingUpdate
```

```bash
helm repo add harbor https://helm.goharbor.io
helm repo update
helm install harbor harbor/harbor -f values.yaml -n harbor --create-namespace
```

```bash
kubectl get pods -n harbor
```
```
output
NAME                                 READY   STATUS             RESTARTS      AGE
harbor-core-c98f9b546-xp8cn          0/1     Running            0             16s
harbor-database-0                    1/1     Running            0             16s
harbor-jobservice-5d464fc78f-hrhr5   0/1     CrashLoopBackOff   1 (14s ago)   16s
harbor-portal-668b998858-ls7sm       1/1     Running            0             16s
harbor-redis-0                       1/1     Running            0             16s
harbor-registry-5f4b876f75-lpmnd     2/2     Running            0             16s
harbor-trivy-0                       1/1     Running            0             16s
```

