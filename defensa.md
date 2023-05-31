# Creación del escenario

Crearemos un nuevo proyecto llamado **"temperaturas"**.
```bash
oc new-project temperaturas
```

Contenido del archivo YAML front-end que define el servicio que proporciona nuestra aplicación, aquí es donde implementaremos la aplicación Serverless con Knative Serving:
```yaml
apiVersion: serving.knative.dev/v1
kind: Service
metadata:
  name: temperaturas-frontend
  labels:
    app: temperaturas
    tier: frontend
spec:
  template:
    spec:
      containers:
        - name: contenedor-temperaturas
          image: iesgn/temperaturas_frontend
          ports:
            - containerPort: 3000
      replicas: 3
```

Contenido del archivo YAML back-end que en este caso es un DeploymentConfig:
```yaml
apiVersion: apps.openshift.io/v1
kind: DeploymentConfig
metadata:
  name: temperaturas-backend
  labels:
    app: temperaturas
    tier: backend
spec:
  replicas: 1
  selector:
    app: temperaturas
    tier: backend
  template:
    metadata:
      labels:
        app: temperaturas
        tier: backend
    spec:
      containers:
        - name: contendor-servidor-temperaturas
          image: iesgn/temperaturas_backend
          ports:
            - name: api-server
              containerPort: 5000
```

Necesitará un servicio para exponer el puerto 5000 del contenedor:
```yaml
apiVersion: v1
kind: Service
metadata:
  name: temperaturas-backend
  labels:
    app: temperaturas
    tier: backend
spec:
  type: ClusterIP
  ports:
  - name: api-server
    port: 5000
    targetPort: api-server
  selector:
    app: temperaturas
    tier: backend
```

Aplicamos los archivos YAML.
```bash
oc apply -f temperaturas-backend-deploymentconfig.yaml
oc apply -f temperaturas-backend-service.yaml
kn service apply -f temperaturas-frontend-service.yaml
```

Comprobamos que se ha creado el servicio serverless para el front-end con el comando:
```bash
kn service list
```

Comprobamos que se han creado los pods, las rutas, los deployments y los servicios con el comando:
```bash
oc get pods,route,deployment,service
```

# Modificación de la app

Hemos cambiado el tiempo de vida de los pods, su escala máxima, su escala mínima y su concurrencia. También hemos creado una nueva revisión, una etiqueta para la misma y hemos cambiado el tráfico entre las revisiones de la app.
```bash
kn service update temperaturas-frontend \
--scale-window 10s \
--scale-min 1 \
--scale-max 5 \
--concurrency-limit 2 \
--scale-target 10 \
--revision-name temperaturas-frontend-v2 \
--traffic @latest=100 \
--tag temperaturas-frontend-v2='final-version'
```

Vamos a volver a cambiar el tráfico.

```bash
 kn service update temperaturas-frontend \
--traffic temperaturas-frontend-00001=50 \
--traffic temperaturas-frontend-v2=50
```

Vamos a inspeccionar la app con los siguientes comando.

```bash
kn revision list
```
```bash
kn route describe temperaturas-frontend
```
```bash
kn service list
```
```bash
oc get pods,route,deployment,service
```
