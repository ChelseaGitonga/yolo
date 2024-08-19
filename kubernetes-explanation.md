## 1. backend-deployment.yaml
This YAML configuration sets up a Kubernetes Deployment and Service for a backend application:

- **Deployment**: Creates a single pod running a container with the image `chelseagitonga/backend-img:v1.0.0`. The pod listens on port 5000 and has resource requests and limits defined. An environment variable is set to connect to a MongoDB service within the cluster.

- **Service**: Exposes the backend pod externally using a LoadBalancer, forwarding traffic from port 5000 to the pod's container. The Service selects the pod based on its label `app: backend`.
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