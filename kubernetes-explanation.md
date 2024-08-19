## 1. backend-deployment.yaml
This YAML configuration sets up a Kubernetes Deployment and Service for a backend application:

- **Deployment**: Creates a single pod running a container with the image `chelseagitonga/backend-img:v1.0.0`. The pod listens on port 5000 and has resource requests and limits defined. An environment variable is set to connect to a MongoDB service within the cluster.

- **Service**: Exposes the backend pod externally using a LoadBalancer, forwarding traffic from port 5000 to the pod's container. The service selects the pod based on its label `app: backend`.
```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: backend-deployment
spec:
  replicas: 1
  selector:
    matchLabels:
      app: backend
  template:
    metadata:
      labels:
        app: backend
    spec:
      containers:
        - name: backend
          image: chelseagitonga/backend-img:v1.0.0
          ports:
            - containerPort: 5000
          env:
            - name: MONGODB_URI
              value: mongodb://mongo-service.default.svc.cluster.local:27017/yolo
          resources:
            requests:
              memory: "128Mi"
              cpu: "250m"
            limits:
              memory: "256Mi"
              cpu: "500m"
---
apiVersion: v1
kind: Service
metadata:
  name: backend-service
spec:
  type: LoadBalancer # This type exposes the service externally using a cloud provider's load balancer
  selector:
    app: backend # The service targets pods with the label app: backend
  ports:
    - protocol: TCP # The communication protocol used
      port: 5000 # The port exposed by the service
      targetPort: 5000 # The port on the pod that the service forwards traffic to. This should match the containerPort defined in the deployment
```

## 2. client-deployment.yaml
This YAML configuration defines a Kubernetes Deployment and Service for a client application.
This configuration deploys a client application as a single pod and exposes it externally via a LoadBalancer on port 3000. The **Deployment** manages the pod's lifecycle, while the **Service** makes the application accessible from outside the Kubernetes cluster.

```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: client-deployment
spec:
  replicas: 1 # Deploys a single pod
  selector:
    matchLabels:
      app: client
  template:
    metadata:
      labels:
        app: client
    spec:
      containers: # Runs a container named `client` using the image `chelseagitonga/client-img:v1.0.0`
        - name: client
          image: chelseagitonga/client-img:v1.0.0
          ports:
            - containerPort: 3000 # The container listens on port 3000
          resources:
            requests: # Minimum memory of 128Mi and CPU of 250m
              memory: "128Mi"
              cpu: "250m"
            limits: # Maximum memory of 256Mi and CPU of 500m
              memory: "256Mi"
              cpu: "500m"
---
apiVersion: v1
kind: Service
metadata:
  name: client-service
spec:
  type: LoadBalancer # This type exposes the service externally using a cloud provider's load balancer
  selector:
    app: client # Targets the pod with the label `app: client`
  ports: # Exposes port 3000, which is mapped to the pod's container port 3000
    - protocol: TCP
      port: 3000
      targetPort: 3000
```