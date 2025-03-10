# Keycloak o'rnatish

## 1. Postgres Baza yaratish 
Docker image ko'tarish
```bash
docker volume create k8s-postgres-data
docker run -d \
  --name k8s-postgres \
  -e POSTGRES_USER=postgres \
  -e POSTGRES_PASSWORD=postgres \
  -e POSTGRES_DB=postgres \
  -p 5433:5432 \
  -v k8s-postgres-data:/var/lib/postgresql/data \
  postgres:16
```
Baza yaratish
```postgresql
CREATE DATABASE keycloak_db;
CREATE USER keycloak_user WITH ENCRYPTED PASSWORD 'keycloak_password';
GRANT ALL PRIVILEGES ON DATABASE keycloak_db TO keycloak_user;
\c keycloak_db
GRANT CREATE ON SCHEMA public TO keycloak_user;
```

## 2. Deployment va service yaml
```yaml
# keycloak-deployment.yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: keycloak
  namespace: keycloak
  labels:
    app: keycloak
spec:
  replicas: 1
  selector:
    matchLabels:
      app: keycloak
  template:
    metadata:
      labels:
        app: keycloak
    spec:
      containers:
        - name: keycloak
          image: quay.io/keycloak/keycloak:26.1.2
          args: ["start-dev"]
          env:
            - name: KEYCLOAK_ADMIN
              value: "admin"
            - name: KEYCLOAK_ADMIN_PASSWORD
              value: "admin"
            - name: KC_PROXY
              value: edge
            - name: KC_DB
              value: postgres
            - name: POSTGRES_DB
              value: keycloak_db
            - name: KC_DB_URL
              value: jdbc:postgresql://host.docker.internal:5433/keycloak_db
            - name: KC_DB_USERNAME
              value: keycloak_user
            - name: KC_DB_PASSWORD
              value: keycloak_password
            - name: KC_HEALTH_ENABLED
              value: "true"
          ports:
            - name: http
              containerPort: 8080
          readinessProbe:
            httpGet:
              path: /health/ready
              port: 9000
---
apiVersion: v1
kind: Service
metadata:
  name: keycloak
  namespace: keycloak
  labels:
    app: keycloak
spec:
  ports:
    - name: http
      port: 8181
      targetPort: 8080
  selector:
    app: keycloak
  type: LoadBalancer
```
## 3. Ingress yaml 
```yaml
# keycloak-ingress.yaml
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: keycloak-ingress
  namespace: keycloak
spec:
  ingressClassName: nginx
  rules:
    - host: keycloak.local
      http:
        paths:
          - path: /
            pathType: Prefix
            backend:
              service:
                name: keycloak
                port:
                  number: 8181
```
## 4 k8sga o'rnatish 
```bash
k create namespace keycloak
k apply -f keycloak-deployment.yaml 
k apply -f keycloak-ingress.yaml
k get pods -n keycloak
```
```
output: 
NAME                        READY   STATUS    RESTARTS   AGE
keycloak-697cc66864-hfgft   1/1     Running   0          1m
```
