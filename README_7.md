Exercise 7 – Kubernetes Deployment and Service for Python App from Docker Hub
Aim: To containerize a Python Flask application, upload its Docker image to Docker Hub, and deploy it using Kubernetes with a NodePort service.

We will follow Exercise 2 (Flask + Docker Hub), with the additional Kubernetes deployment and service steps.

Step 1: Create the Flask application
Open PowerShell:

mkdir python-k8s-app
cd python-k8s-app
notepad app.py
Paste this code into app.py:
(paste from 2)
from flask import Flaskapp = Flask(__name__)@app.route('/')def home():    return "Hello! Python App running on Kubernetes."@app.route('/about')def about():    return "This Flask application is deployed using Kubernetes."if __name__ == '__main__':    app.run(host='0.0.0.0', port=5000)
Step 2: Create requirements.txt
notepad requirements.txt
Paste:


Flask==3.0.3
Step 3: Create Dockerfile
notepad Dockerfile
Paste:


FROM python:3.11-slim

WORKDIR /app

COPY requirements.txt .

RUN pip install --no-cache-dir -r requirements.txt

COPY app.py .

EXPOSE 5000

CMD ["python", "app.py"]
Step 4: Build the Docker image
docker build -t python-k8s-app .
docker images
Step 5: Upload the image to Docker Hub
Login:

docker login
Tag the image:

docker tag python-k8s-app klassy01/python-k8s-app:latest
Push the image:

docker push klassy01/python-k8s-app:latest
Make sure the Docker Hub repository python-k8s-app exists and is public.

Step 6: Start Kubernetes using Minikube
Make sure Docker Desktop is running.

minikube start --driver=docker
minikube status
kubectl get nodes
The node should show Ready.

Step 7: Create a Kubernetes Deployment
Use the image uploaded to Docker Hub:

kubectl create deployment python-app --image=klassy01/python-k8s-app:latest --port=5000
Check the deployment and Pods:

kubectl get deployments
kubectl get pods
kubectl describe deployment python-app
Wait until the Pod status becomes Running.

Step 8: Create a Kubernetes Service
Expose the deployment using NodePort:

kubectl expose deployment python-app --type=NodePort --port=5000 --target-port=5000
Check the service:

kubectl get services
kubectl describe service python-app
The service forwards traffic from its exposed port to port 5000 inside the Flask container.

Kubernetes+1
Step 9: Access the application
Run:

minikube service python-app --url
Copy the URL displayed and open it in your browser.

For example, if it displays:

http://127.0.0.1:54321

Open that URL.

To test the second route, add /about to the URL.

Expected output
Home page: Hello! Python App running on Kubernetes.

About page: This Flask application is deployed using Kubernetes.

Important: Keep the PowerShell window running while using the URL. Minikube may need to maintain a tunnel on Windows.

minikube+1
Step 10: Clean up
kubectl delete service python-app
kubectl delete deployment python-app
minikube stop
