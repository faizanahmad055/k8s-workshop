# k8s-workshop
This repository contains the sample code needed for workshop

## Pre-requisites
- Install Docker
- Install kubectl
- Install minikube
- Install helm

## Steps
1. Start minikube
```bash
minikube start
```

2. Enable ingress
```bash
minikube addons enable ingress
```

3. Setup local DNS
Add the following entry to your `/etc/hosts` file
```bash
127.0.0.1 your-backend-domain.com
```

4. Build the docker images
```bash
docker build -t faizanahmad055/backend:0.0.1 -f Dockerfile . --platform="linux/amd64"
docker build -t faizanahmad055/frontend:0.0.1 -f Dockerfile . --platform="linux/amd64"
```

5. Upload the images to minikube
```bash
minikube image load docker.io/faizanahmad055/frontend:0.0.1
minikube image load docker.io/faizanahmad055/backend:0.0.1
```

6. Create a namespace
```bash
kubectl create namespace test
```

7. Deploy the application
```bash
kubectl apply -f backend/kubernetes -n test
kubectl apply -f frontent/kubernetes -n test
```

8. Access the application
```bash
kubectl port-forward service/frontend 8082:80 -n test
kubectl port-forward service/backend 8080:8080 -n test
```

9. Delete the application
```bash
kubectl delete -f backend/kubernetes -n test
kubectl delete -f frontent/kubernetes -n test
```

10. Deploy the application using helm
```bash
helm install be backend/helm/backend -n test
helm install fe frontend/helm/frontend -n test
```

11. Install Prometheus
```bash
helm install my-release oci://registry-1.docker.io/bitnamicharts/kube-prometheus
```

12. Install Grafana
```bash
helm install my-grafana oci://registry-1.docker.io/bitnamicharts/grafana-operator
```

13. Add datasource to Grafana
```bash
kubectl apply -f grafana/datasource.yaml
```

14. Port forward Grafana
```bash
kubectl port-forward svc/my-grafana-grafana-operator-grafana-service 3000:3000
```

15. Get Grafana password
```bash
kubectl get secret my-grafana-grafana-operator-grafana-admin-credentials --namespace default -o jsonpath="{.data.GF_SECURITY_ADMIN_PASSWORD}" | base64 -d
```

16. Access Grafana
Go to http://localhost:3000 and use admin as username and password gotten from step 15

17. Import dashboard
Go to https://grafana.com/grafana/dashboards/19004-spring-boot-statistics/ and copy the dashboard id
Go to http://localhost:3000/dashboard/import and paste the dashboard id

