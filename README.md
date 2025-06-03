# manifest-folder

├── manifests/
│   ├── nginx-deployment.yaml
│   └── nginx-service.yaml
├── argocd/
│   └── nginx-app.yaml
├── README.md


kubectl apply -f manifests/nginx-deployment.yaml
kubectl apply -f manifests/nginx-service.yaml
kubectl get deployments
kubectl get services

Deploying with ArgoCD
 create file nginx-app.yaml

Replace <your-username> and <your-repo> with your GitHub username and repository name.
: Using kubectl port-forward
kubectl apply -f argocd/nginx-app.yaml
Access the application with  http://localhost:8080.
for clean up
kubectl delete -f manifests/nginx-deployment.yaml
kubectl delete -f manifests/nginx-service.yaml
kubectl delete -f argocd/nginx-app.yaml


