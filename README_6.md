Exercise 6 – Kubernetes using Minikube
Aim: To deploy and manage applications using Kubernetes commands.

Step 1: Check Docker, kubectl and Minikube
Open PowerShell as Administrator.

docker --version
docker ps
kubectl version --client
minikube version
If kubectl or Minikube is not installed:

choco install kubernetes-cli -y
choco install minikube -y
Step 2: Start Minikube
minikube start --driver=docker
minikube status
kubectl get nodes
Expected: The Minikube node should show Ready.

Step 3: Create a Kubernetes Pod
mkdir kubernetes-lab
cd kubernetes-lab
notepad nginx-pod.yaml
Paste this YAML into the file:


apiVersion: v1
kind: Pod
metadata:
  name: nginx-pod
  labels:
    app: nginx
spec:
  containers:
  - name: nginx
    image: nginx:latest
    ports:
    - containerPort: 80
Save the file, then run:

kubectl apply -f nginx-pod.yaml
kubectl get pods
kubectl describe pod nginx-pod
Step 4: Access the Pod
kubectl port-forward pod/nginx-pod 8080:80
Open this URL in your browser:

http://localhost:8080

Press Ctrl + C in PowerShell to stop port forwarding.

Step 5: Delete the Pod
kubectl delete pod nginx-pod
kubectl get pods
Step 6: Create a Deployment
kubectl create deployment my-nginx --image=nginx
kubectl get deployments
kubectl get pods
Step 7: Scale the Deployment
Create three replicas:

kubectl scale deployment my-nginx --replicas=3
kubectl get deployments
kubectl get pods
Expected: Three Nginx Pods should be running.

Step 8: Perform a Rolling Update
kubectl set image deployment/my-nginx nginx=nginx:1.25
kubectl rollout status deployment/my-nginx
kubectl get pods
Step 9: Rollback the Deployment
kubectl rollout undo deployment/my-nginx
kubectl rollout status deployment/my-nginx
Step 10: Expose the Deployment
kubectl expose deployment my-nginx --type=NodePort --port=80
kubectl get services
minikube service my-nginx --url
Open the URL displayed in PowerShell.

Step 11: Clean up
kubectl delete service my-nginx
kubectl delete deployment my-nginx
minikube stop
