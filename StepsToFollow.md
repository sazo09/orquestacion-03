# Guía de comandos - Ejercicios de Kubernetes

## Ejercicio 00: Monolito en memoria

### 1. Iniciar Minikube
```sh
minikube start
```

### 2. Aplicar recursos
```sh
cd exercises/00-monolith-in-mem
kubectl apply -f deployment.yaml
```

### 3. Verificar estado
```sh
kubectl get pods
kubectl get svc
```

### 4. Acceder a la aplicación
**Opción 1: Port-forward**
```sh
kubectl port-forward svc/todo-app-service 3000:3000
```
Luego accede a: `http://localhost:3000`

**Opción 2: Minikube service**
```sh
minikube service todo-app-service
```

---

## Ejercicio 01: Monolito con persistencia (PostgreSQL)

### 1. Iniciar Minikube
```sh
minikube start
```

### 2. Aplicar recursos de almacenamiento
```sh
cd exercises/01-monolith
kubectl apply -f storage.yaml
```

### 3. Aplicar ConfigMap de PostgreSQL
```sh
kubectl apply -f postgres-config.yaml
```

### 4. Aplicar StatefulSet de PostgreSQL
```sh
kubectl apply -f postgres-statefulset.yaml
```

### 5. Aplicar seed (ConfigMap + Job)
```sh
kubectl apply -f todos-seed-configmap.yaml
kubectl apply -f todos-seed-job.yaml
```

### 6. Esperar a que el seed se complete
```sh
kubectl get jobs
kubectl logs job/seed-todos-db
```

### 7. Aplicar aplicación
```sh
kubectl apply -f todo-app-config.yaml
kubectl apply -f todo-app-deployment.yaml
kubectl apply -f loadbalancer.yaml
```

### 8. Verificar estado
```sh
kubectl get pods
kubectl get svc
```

### 9. Acceder a la aplicación
**Con Minikube tunnel:**
```sh
minikube tunnel
```
Luego accede a: `http://localhost`

**O con port-forward:**
```sh
kubectl port-forward svc/todo-app-lb 8080:80
```
Luego accede a: `http://localhost:8080`

---

## Ejercicio 02: Arquitectura distribuida (Frontend + API + Ingress)

### 1. Iniciar Minikube
```sh
minikube start
```

### 2. Habilitar NGINX Ingress Controller
```sh
minikube addons enable ingress
```

### 3. Aplicar Frontend
```sh
cd exercises/02-distributed/todo-front
kubectl apply -f frontend-deployment.yaml
kubectl apply -f frontend-service.yaml
```

### 4. Aplicar API
```sh
cd exercises/02-distributed/todo-api
kubectl apply -f api-config.yaml
kubectl apply -f api-deployment.yaml
kubectl apply -f api-service.yaml
```

### 5. Aplicar Ingress
```sh
cd exercises/02-distributed
kubectl apply -f ingress.yaml
```

### 6. Verificar estado
```sh
kubectl get pods
kubectl get svc
kubectl get ingress
```

### 7. Acceder a los servicios
**Con Minikube tunnel:**
```sh
minikube tunnel
```
Luego accede a:
- Frontend: `http://localhost/`
- API: `http://localhost/api`

**O con port-forward:**
```sh
kubectl port-forward svc/todo-front-service 8080:80
kubectl port-forward svc/todo-api-service 3001:3000
```
Luego accede a:
- Frontend: `http://localhost:8080`
- API: `http://localhost:3001`

---

## Comandos útiles

### Ver logs
```sh
kubectl logs <pod-name>
kubectl logs -f <pod-name>  # tail -f
```

### Ver descripción de un pod
```sh
kubectl describe pod <pod-name>
```

### Eliminar todos los recursos
```sh
kubectl delete all --all
```

### Detener Minikube
```sh
minikube stop
```

### Reiniciar Minikube
```sh
minikube delete
minikube start
```