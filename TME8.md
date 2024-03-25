# TME 8

Correction du TME 7 :

On doit créer 4 entités

```yaml
#redis-service.yml
apiVersion: v1
kind: Service
metadata:
  name: redis
spec:
  selector:
    app: redis
  ports:
    - port: 6379
      targetPort: 6379
  type: ClusterIP
```

```yaml
#redis-deployment.yml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: redis
  labels:
    app: redis
spec:
  replicas: 1
  selector:
    matchLabels:
      app: redis
  template:
    metadata:
      labels:
        app: redis
    spec:
      containers:
        - name: redis
          image: redis:7.2
          ports:
            - containerPort: 6379
```

Ces deux éléments permettent de déployer et exposer une base redis.

```yaml
#node-redis-service.yml
apiVersion: v1
kind: Service
metadata:
  name: node-redis
spec:
  selector:
    app: node-redis
  ports:
    - port: 8080
      targetPort: 8080
  type: LoadBalancer
```

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: node-redis
  labels:
    app: node-redis
spec:
  replicas: 1
  selector:
    matchLabels:
      app: node-redis
  template:
    metadata:
      labels:
        app: node-redis
    spec:
      containers:
        - name: node-redis
          imagePullPolicy: Always
          image: arthurescriou/node-redis:1.0.5
          ports:
            - containerPort: 8080
          env:
            - name: PORT
              value: '8080'
            - name: REDIS_URL
              value: redis://redis.default.svc.cluster.local:6379
            - name: REDIS_REPLICAS_URL
              value: redis://redis.default.svc.cluster.local:6379
```

Ces deux éléments permettent de déployer le serveur.

L'url `redis.default.svc.cluster` permet d'accéder à un service depuis l'intérieur du cluster kubernetes.

### Replicas

Pour la suite on veut dupliquer la base redis (comme dans le docker compose du TME7) avec des redis replicas qui s'enregistre sur la base redis principale.
