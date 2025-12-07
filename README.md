# 1. Utworzenie klastra Kubernetes z 4 węzłami
```
minikube start --nodes=4 -p lab --cni=calico  --container-runtime=docker 
```
# Dodanie etykiet dla węzłów
```
kubectl label node lab-m02 node=frontend
kubectl label node lab-m03 node=backend
kubectl label node lab-m04 node=db
```
# 2. Tworzenie Deploymentów i Poda z przypisaniem do węzłów
```
kubectl create -f deployment-frontend.yaml
kubectl create -f deployment-backend.yaml
kubectl create -f pod-mysql.yaml
```
# 3-4. Tworzenie Service'ów
```
kubectl create -f service-frontend.yaml
kubectl create -f service-backend.yaml
kubectl create -f service-mysql.yaml
```
# 5. Tworzenie Network Policy
```
kubectl create -f networkpolicy-mysql.yaml
```
# 6. Sprawdzenie poprawnośći działania opracowanej polityki sieciowej
## W celu sprawdzenia poprawności działania polityki sieciowej użyto tymczasowych Podów busybox z odpowiednimi label:
### 1. Test z “frontend” (dostęp zablokowany)
```
kubectl run test-frontend --rm -it --image=busybox --restart=Never --labels="app=frontend" -- sh
# W shellu testujemy połączenie do my-sql
nc -zv my-sql 3306
```
### 2. Test z “backend” (dostęp dozwolony)
```
kubectl run test-backend --rm -it --image=busybox --restart=Never --labels="app=backend" -- sh
# W shellu testujemy połączenie do my-sql
nc -zv my-sql 3306
```
