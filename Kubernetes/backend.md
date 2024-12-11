### without LoadBalancer

```bash

apiVersion: apps/v1
kind: Deployment
metadata:
 name: backend
spec:
 selector:
   matchLabels:
     run: backend
 replicas: 1
 template:
   metadata:
     labels:
       run: backend
   spec:
     containers:
     - name: backend
       image: jibrandev/backend:latest
       ports:
       - containerPort: 80
       resources:
         limits:
           cpu: 500m
         requests:
           cpu: 200m

---
apiVersion: v1
kind: Service
metadata:
  name: backend
  labels:
    run: backend
spec:
  type: NodePort  # This exposes the service outside the cluster
  ports:
    - port: 80      # The port that the service will expose inside the cluster
      targetPort: 80 # The port that the Pod's container listens on
      nodePort: 30080 # Optional: Specify a NodePort. If omitted, Kubernetes will auto-assign one
  selector:
    run: backend


---
apiVersion: autoscaling/v1
kind: HorizontalPodAutoscaler
metadata:
 name: backend
spec:
 scaleTargetRef:
   apiVersion: apps/v1
   kind: Deployment
   name: backend
 minReplicas: 1
 maxReplicas: 10
 targetCPUUtilizationPercentage: 10

```

 ### with load balancer

```bash


 apiVersion: apps/v1
kind: Deployment
metadata:
  name: backend
spec:
  selector:
    matchLabels:
      run: backend
  replicas: 1
  template:
    metadata:
      labels:
        run: backend
    spec:
      containers:
        - name: backend
          image: backend
          ports:
            - containerPort: 80
          resources:
            limits:
              cpu: 500m
            requests:
              cpu: 200m

---
apiVersion: v1
kind: Service
metadata:
  name: backend
  labels:
    run: backend
spec:
  type: LoadBalancer  # LoadBalancer service for production
  ports:
    - port: 80      # Port exposed inside the cluster
      targetPort: 80 # The container's listening port
  selector:
    run: backend

---
apiVersion: autoscaling/v1
kind: HorizontalPodAutoscaler
metadata:
  name: backend
spec:
  scaleTargetRef:
    apiVersion: apps/v1
    kind: Deployment
    name: backend
  minReplicas: 1
  maxReplicas: 10
  targetCPUUtilizationPercentage: 10
```