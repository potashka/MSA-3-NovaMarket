# Task3 Verification Commands

```bash
minikube start
minikube addons enable metrics-server

kubectl apply -f Task3/deployment.yaml
kubectl apply -f Task3/service.yaml
kubectl apply -f Task3/hpa.yaml

kubectl get pods
kubectl get hpa

kubectl port-forward svc/scaletestapp 8080:8080

locust

kubectl get hpa -w
kubectl top pods

minikube dashboard
```

Скриншоты и логи HPA нужно добавить вручную после тестирования в `Task3/evidence`.
