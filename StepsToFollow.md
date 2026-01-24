# Pasos del ejercicio "Monolito en memoria" (Kubernetes y orquestación)

Basado en el enunciado del archivo `exercise-monolith-in-memory.md`, el objetivo es desplegar una aplicación monolítica en memoria usando Kubernetes. Aquí se detallan los pasos principales para ordenar las ideas, sin crear nada aún. Puedes investigar cada uno por separado antes de implementar.

## 1. Preparar la imagen de Docker para todo-app

- Usa el `Dockerfile` ubicado en `todo-app` para construir la imagen de la aplicación.
- **Alternativa:** Usa la imagen pre-construida `lemoncodersbc/lc-todo-monolith:v5-2024`.
- La aplicación expone el puerto `3000` por defecto, configurable vía variables de entorno:
    - `NODE_ENV`: Cualquier valor excepto `test` (ej. `production` o `development`).
    - `PORT`: El puerto deseado (ej. `3000`).

Pasos

- docker build -t todo-app:v1 .
- docker run -e PORT=3000 -e NODE_ENV=production -p 3000:3000 todo-app:v1

## 2. Crear un Deployment de Kubernetes

- Define un Deployment en un archivo YAML (ej. `deployment.yaml`) que despliegue la imagen de Docker.
- Especifica las variables de entorno en el `spec` del contenedor.
- Asegúrate de que el Deployment maneje réplicas (inicialmente 1) y el puerto correcto.

## 3. Crear un Service de tipo LoadBalancer

- Define un Service en otro archivo YAML (ej. `service.yaml`) que exponga el Deployment.
- Usa `type: LoadBalancer` para acceso externo.
- Para minikube (asumiendo que usas este entorno local), configura el LoadBalancer siguiendo el artículo mencionado: *Accediendo a servicios en minikube*. Esto implica comandos como `minikube tunnel` para exponer el servicio.

## 4. Aplicar y verificar en el clúster

- Una vez definidos los YAML, aplícalos con `kubectl apply`.
- Verifica el estado con comandos como `kubectl get pods`, `kubectl get services`, etc.
- Accede a la aplicación desde fuera del clúster usando la IP/puerto asignado por el LoadBalancer.