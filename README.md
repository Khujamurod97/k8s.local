# Maqsad 
Bu repository kubernetes (k8s) ni o'rganish jarayonida ishga tushirilgan buyruqlar va yaratilgan yaml fayllarni saqlab qo'yish uchun ochildi. 
Har bir papka bitta namespace ni o'z ichiga oladi 

# K8s ni local o'rnatish. 
Buning uchun docker Desktop ilovasi o'rnatilda va Settings bo'limidan Enable Kubernetes yoqib qo'yildi. 
<img width="1259" alt="image" src="https://github.com/user-attachments/assets/6791e11d-0aac-407a-9bd2-5b525c39f586" />

Natijada kompyuterga k8s o'rnatildi, helm va kubeclt avtomat nastroyka qilindi. K8s o'rnatishni boshqa yo'li Minikube o'rnatish. Ikkala variant ham kompyuterga single-note cluster o'rnatib beradi va o'ganish uchun yetarli. 

# Kubeclt alias
```bash
alias k=kubectl
```

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

### Mertic Server commands 
- Note larni ko'rish
```
kubeclt top node
```
- Hamma podlarni ko'rish
```
kubectl top pod -A
```
- Hamma podlarni hotiradan joy olishi bo'yicha saralash
```
kubectl top pod -A --sort-by=memory
```
- Hamma podlarni cpudan joy olishi bo'yicha saralash
```
kubectl top pod -A --sort-by=cpu
```
