# Maqsad 
Bu repository kubernetes (k8s) ni o'rganish jarayonida ishga tushirilgan buyruqlar va yaratilgan yaml fayllarni saqlab qo'yish uchun ochildi. 
Har bir papka bitta namespace ni o'z ichiga oladi 

# K8s ni local o'rnatish.
[K8s Dockerga o'rnatish](Docker-K8s.md).

# Kubeclt alias
```bash
alias k=kubectl
```

# Metric Server
Mertic Server bizga node va pod larni (xotira va cpu dan qancha joy olishini) kutatish uchun kerak bo'ladi

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
[Mertic Server o'rnatish](kube-system/Mertic-Server.md).
