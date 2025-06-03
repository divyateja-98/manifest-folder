# manifest-folder

├── manifests/
│   ├── nginx-deployment.yaml
│   └── nginx-service.yaml
├── argocd/
│   └── nginx-app.yaml
├── README.md


create a file nginx-deployment.yaml   
 apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
  labels:
    app: nginx
spec:
  replicas: 2
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
        - name: nginx
          image: nginx:latest
          ports:
            - containerPort: 80

    create nginx-service.yaml  
     apiVersion: v1
kind: Service
metadata:
  name: nginx-service
  labels:
    app: nginx
spec:
  type: NodePort
  selector:
    app: nginx
  ports:
    - protocol: TCP
      port: 80
      targetPort: 80
      nodePort: 30080
kubectl apply -f manifests/nginx-deployment.yaml
kubectl apply -f manifests/nginx-service.yaml
kubectl get deployments
kubectl get services

Deploying with ArgoCD
 create file nginx-app.yaml
 apiVersion: argoproj.io/v1alpha1
kind: Application
metadata:
  name: nginx-app
  namespace: argocd
spec:
  project: default
  source:
    repoURL: 'https://github.com/<your-username>/<your-repo>.git'
    targetRevision: HEAD
    path: manifests
  destination:
    server: 'https://kubernetes.default.svc'
    namespace: default
  syncPolicy:
    automated:
      prune: true
      selfHeal: true
Replace <your-username> and <your-repo> with your GitHub username and repository name.
: Using kubectl port-forward
kubectl apply -f argocd/nginx-app.yaml
Access the application with  http://localhost:8080.
for clean up
kubectl delete -f manifests/nginx-deployment.yaml
kubectl delete -f manifests/nginx-service.yaml
kubectl delete -f argocd/nginx-app.yaml


