# Metric Server o'rnatish
Mertic Server bizga node va pod larni (xotira va cpu dan qancha joy olishini) kutatish uchun kerak bo'ladi

### Metric servier bor yoki yo'qligini tekshirish
```bash
kubectl get deployment -n kube-system | grep metrics-server
```
Agar outputda hech qanday natija chiqmasa demak mertic server o'rnatilmagan

### Mertic Serverni o'rnatish
```bash
kubectl apply -f https://github.com/kubernetes-sigs/metrics-server/releases/latest/download/components.yaml
```

### Insecure tls ga ruxsat berish
Birlamchi holatda valid sertifikat bo'lishi kerak. buni patch orqali o'chirib qo'yamiz
```bash
kubectl patch deployment metrics-server -n kube-system \
  --type='json' -p='[{"op": "add", "path": "/spec/template/spec/containers/0/args/-", "value": "--kubelet-insecure-tls"}]'
```

### Mertic Server ishlayotganini tekshirish
```bash
kubectl get pods -n kube-system | grep metrics-server
```
```
Output
metrics-server-587b667b55-j8mbp          1/1     Running   0          1m
```

## Mertic Server commands
### Note larni ko'rish
```bash
kubeclt top node
```
```
NAME             CPU(cores)   CPU%   MEMORY(bytes)   MEMORY%   
docker-desktop   186m         2%     1976Mi          23%   
```

### Hamma podlarni ko'rish
```bash
kubectl top pod -A
```
```
NAMESPACE     NAME                                     CPU(cores)   MEMORY(bytes)   
kube-system   coredns-7c65d6cfc9-b8p42                 3m           18Mi            
kube-system   coredns-7c65d6cfc9-m4fwd                 3m           16Mi            
kube-system   etcd-docker-desktop                      23m          44Mi            
kube-system   kube-apiserver-docker-desktop            36m          203Mi           
kube-system   kube-controller-manager-docker-desktop   26m          55Mi            
kube-system   kube-proxy-p9t76                         2m           21Mi            
kube-system   kube-scheduler-docker-desktop            4m           22Mi            
kube-system   metrics-server-587b667b55-j8mbp          5m           24Mi            
kube-system   storage-provisioner                      4m           14Mi            
kube-system   vpnkit-controller                        1m           11Mi     
```

### Hamma podlarni hotiradan joy olishi bo'yicha saralash
```bash
kubectl top pod -A --sort-by=memory
```
```
NAMESPACE     NAME                                     CPU(cores)   MEMORY(bytes)   
kube-system   kube-apiserver-docker-desktop            34m          203Mi           
kube-system   kube-controller-manager-docker-desktop   20m          55Mi            
kube-system   etcd-docker-desktop                      25m          44Mi            
kube-system   metrics-server-587b667b55-j8mbp          4m           24Mi            
kube-system   kube-scheduler-docker-desktop            4m           22Mi            
kube-system   kube-proxy-p9t76                         2m           21Mi            
kube-system   coredns-7c65d6cfc9-b8p42                 3m           18Mi            
kube-system   coredns-7c65d6cfc9-m4fwd                 3m           16Mi            
kube-system   storage-provisioner                      3m           14Mi            
kube-system   vpnkit-controller                        1m           11Mi   
```
### Hamma podlarni cpudan joy olishi bo'yicha saralash
```bash
kubectl top pod -A --sort-by=cpu
```
```
NAMESPACE     NAME                                     CPU(cores)   MEMORY(bytes)   
kube-system   kube-apiserver-docker-desktop            36m          203Mi           
kube-system   etcd-docker-desktop                      23m          44Mi            
kube-system   kube-controller-manager-docker-desktop   22m          55Mi            
kube-system   kube-scheduler-docker-desktop            5m           22Mi            
kube-system   coredns-7c65d6cfc9-m4fwd                 4m           16Mi            
kube-system   metrics-server-587b667b55-j8mbp          4m           24Mi            
kube-system   coredns-7c65d6cfc9-b8p42                 3m           18Mi            
kube-system   storage-provisioner                      3m           14Mi            
kube-system   kube-proxy-p9t76                         1m           21Mi            
kube-system   vpnkit-controller                        1m           11Mi        
```