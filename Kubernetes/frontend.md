### without LoadBalancer

```bash

apiVersion: apps/v1
kind: Deployment
metadata:
 name: frontend
spec:
 selector:
   matchLabels:
     run: frontend
 replicas: 1
 template:
   metadata:
     labels:
       run: frontend
   spec:
     containers:
     - name: frontend
       image: jibrandev/frontend:latest
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
  name: frontend
  labels:
    run: frontend
spec:
  type: NodePort  # This exposes the service outside the cluster
  ports:
    - port: 80      # The port that the service will expose inside the cluster
      targetPort: 80 # The port that the Pod's container listens on
      nodePort: 30080 # Optional: Specify a NodePort. If omitted, Kubernetes will auto-assign one
  selector:
    run: frontend


---
apiVersion: autoscaling/v1
kind: HorizontalPodAutoscaler
metadata:
 name: frontend
spec:
 scaleTargetRef:
   apiVersion: apps/v1
   kind: Deployment
   name: frontend
 minReplicas: 1
 maxReplicas: 10
 targetCPUUtilizationPercentage: 10

```

 ### with load balancer

```bash


 apiVersion: apps/v1
kind: Deployment
metadata:
  name: frontend
spec:
  selector:
    matchLabels:
      run: frontend
  replicas: 1
  template:
    metadata:
      labels:
        run: frontend
    spec:
      containers:
        - name: frontend
          image: frontend
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
  name: frontend
  labels:
    run: frontend
spec:
  type: LoadBalancer  # LoadBalancer service for production
  ports:
    - port: 80      # Port exposed inside the cluster
      targetPort: 80 # The container's listening port
  selector:
    run: frontend

---
apiVersion: autoscaling/v1
kind: HorizontalPodAutoscaler
metadata:
  name: frontend
spec:
  scaleTargetRef:
    apiVersion: apps/v1
    kind: Deployment
    name: frontend
  minReplicas: 1
  maxReplicas: 10
  targetCPUUtilizationPercentage: 10
```