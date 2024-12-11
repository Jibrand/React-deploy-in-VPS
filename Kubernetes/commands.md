# Kubernetes and Docker Notes

## Forcefully Delete Deployment Content in kubectl
```bash
kubectl delete pod react-app-887cf4f66-s4wmz --grace-period=0 --force
```

## Service Management
Get service information:
```bash
kubectl get svc react-app-service
```

## Steps to Update Deployment After Kubernetes Deployment
1. Update your code.
2. Build a new Docker image:
   ```bash
   docker build -t jibrandev/frontend:v1.1 .
   ```
3. Push the image to your registry:
   ```bash
   docker push jibrandev/frontend:v1.1
   ```
4. Update the deployment image:
   ```bash
   kubectl set image deployment/frontend-deployment frontend=jibrandev/frontend:v1.1 -n mern
   ```
5. Check the rollout status:
   ```bash
   kubectl rollout status deployment/frontend-deployment -n mern
   ```

## Kubernetes Rollback Command
To rollback the deployment:
```bash
kubectl rollout undo deployment/frontend-deployment
```

## Steps to Conduct Load Tests
### Select a Load Testing Tool:
1. **Apache JMeter**: Open-source tool for performance and load testing.
2. **Gatling**: A high-performance, open-source load testing tool.
3. **Artillery**: A modern, powerful, and easy-to-use load testing toolkit.
4. **k6**: Open-source load testing tool that allows you to write tests in JavaScript.

## Scale Up Manually
To manually scale the deployment:
```bash
kubectl scale deployment nginx --replicas=1
```

## Horizontal Pod Autoscaler (HPA)
Check HPA status:
```bash
kubectl get hpa
```
### Metric Server (Necessary for HPA)
Run all resources from the repository:
[Metrics Server GitHub](https://github.com/ashokitschool/k8s_metrics_server/tree/main/deploy/1.8%2B)

## Docker Registry
### Configure Insecure Registry
Edit the Docker daemon configuration file:
```bash
vi /etc/docker/daemon.json
```
Add the following:
```json
{
  "insecure-registries": ["192.168.145.130:5000"]
}
```
Restart Docker if needed.

### Run a Private Docker Registry
Start the registry:
```bash
docker run -d -p 5000:5000 --restart=always --name registry -v $(pwd)/dregistry:/var/lib/registry registry:latest
```
Test the registry:
```bash
curl -X GET http://192.168.145.130:5000/v2/_catalog
```

### Push Image to Registry
```bash
docker tag react-app 172.31.41.90:5000/react-app
docker push 172.31.41.90:5000/react-app
```

## Pull Images from Private Registry in Kubernetes
Create a Docker registry secret:
```bash
kubectl create secret docker-registry regcred \
  --docker-server=d.genieengage.tech \
  --docker-username=<your-username> \
  --docker-password=<your-password>
```

### Add Secret to Deployment YAML File
```yaml
spec:
  containers:
  - name: react-app
    image: d.genieengage.tech/react-app
    ports:
    - containerPort: 3000
  imagePullSecrets:
  - name: regcred
```
