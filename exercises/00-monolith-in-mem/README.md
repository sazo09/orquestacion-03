# Todo App React

## Run solution locally

First we need to install dependencies change directory to `todo-app/frontend` and run `npm install`, then change directory to `/todo-app` and run `npm install`. Once that all dependencies are installed, we can run the solution locally by changing directory to `todo-app/frontend` and running `npm run start:dev:server`.

```bash
# 1. Install frontend dependencies
cd ./todo-app/frontend
npm install

# 2. Install backend dependencies
cd ../
mpm install

# 3. To start solution run the following command
npm run start:dev:server
```

## Environment Variables

```ini
NODE_ENV=
PORT=
```

## Running the Application with Docker on Local

```bash
docker build -t jaimesalas/lc-todo-monolith . 
```

Start app without database

```bash
docker run -d -p 3000:3000 \
  -e NODE_ENV=production \
  -e PORT=3000 \
  jaimesalas/lc-todo-monolith
```

## Running the Application with Minikube

> **Importante:** Ejecuta todos los comandos desde la carpeta `exercises/00-monolith-in-mem` para evitar rutas largas.

```bash
cd exercises/00-monolith-in-mem
```

### Prerrequisitos

- Minikube instalado y corriendo
- kubectl configurado

```bash
# Verificar que Minikube está corriendo
minikube status

# Si no está corriendo, iniciarlo
minikube start
```

### Paso 1: Construir la imagen en Minikube

Minikube tiene su propio Docker daemon, por lo que debemos construir la imagen dentro de él:

```bash
# Construir la imagen directamente en Minikube
minikube image build -t todo-app:v1 ./todo-app
```

### Paso 2: Desplegar la aplicación

```bash
# Aplicar el Deployment
kubectl apply -f deployment.yaml

# Verificar que el pod está corriendo
kubectl get pods
```

### Paso 3: Exponer la aplicación con LoadBalancer

```bash
# Aplicar el Service LoadBalancer
kubectl apply -f loadbalancer.yaml

# Verificar el servicio
kubectl get svc
```

### Paso 4: Crear el túnel de Minikube

Para que el LoadBalancer funcione en local, necesitas ejecutar `minikube tunnel` en una **terminal separada**:

```bash
# En una terminal SEPARADA (mantenerla abierta)
minikube tunnel
```

> ⚠️ Este comando pedirá permisos de administrador y debe permanecer ejecutándose.

### Paso 5: Acceder a la aplicación

```bash
# Obtener la IP externa
kubectl get svc todo-app-service
```

Accede en tu navegador a: `http://EXTERNAL-IP:80`

### Comandos útiles

```bash
# Ver logs del pod
kubectl logs -l app=todo-app

# Ver detalles del deployment
kubectl describe deployment todo-app

# Eliminar todos los recursos
kubectl delete -f deployment.yaml
kubectl delete -f loadbalancer.yaml
```