Exercise 2 – Flask App Containerization and Docker Hub Deployment
Tools: Python, Flask, Docker, Docker Hub, VS Code.

Step 1: Create project folder
Open PowerShell:

mkdir flask-docker-app
cd flask-docker-app
Open the folder in VS Code:

code .
Step 2: Create app.py
notepad app.py
Paste:

from flask import Flask, render_template_string

app = Flask(__name__)

HTML_TEMPLATE = """
<!DOCTYPE html>
<html>
<head>
<title>Flask Docker App</title>
<style>
    body {
        background:linear-gradient(120deg,blue,purple);
        color:white;
        padding:100px;
        text-align:center;
        font-family:Arial;
    }
    .card{
        background:white;
        color:black;
        padding:30px;
        border-radius:20px;
        font-family:Arial;
        text-align:center;
        display:inline-block;
    }
</style>
</head>
<body>
   <dev class = "card">
     <h1> Welcome to Flask + Docker app </h1>
     <p> This application is being run by Docker </p>
    </dev>
</body>
<html>
"""

@app.route('/')
def home():
    return render_template_string(HTML_TEMPLATE)

if __name__ == '__main__':
    app.run(host="0.0.0.0", port="5000")



Step 3: Create requirements.txt
notepad requirements.txt
Paste:

Flask==2.2.5
Save and close.

Step 4: Create Dockerfile
notepad Dockerfile
Paste:


FROM python:3.9-slim

WORKDIR /app

COPY requirements.txt .

RUN pip install --no-cache-dir -r requirements.txt

COPY app.py .

EXPOSE 5000

CMD ["python", "app.py"]
Save and close.

Step 5: Build Docker Image
docker build -t flask-docker-app .
Check image:

docker images
Step 6: Run Docker Container
docker run -d --name flask-container -p 5000:5000 flask-docker-app
Check running container:

docker ps
Step 7: Test Flask Application
Open Chrome:

Main page: http://localhost:5000

About page: http://localhost:5000/about

Expected output:

Hello! Welcome to Flask Docker Application.
About page:

This is a Flask application running inside Docker.
Step 8: Run Application on Another Port (5050)
Stop the first container:

docker stop flask-container
docker rm flask-container
Run using port 5050:

docker run -d --name flask-container -p 5050:5000 flask-docker-app
Open:

http://localhost:5050
Step 9: Login to Docker Hub
docker login
Enter your Docker Hub username and password/token when prompted.

Step 10: Tag Docker Image
Using your Docker Hub username klassy01:

docker tag flask-docker-app klassy01/flask-docker-app:latest
Check:

docker images
Step 11: Push Image to Docker Hub
docker push klassy01/flask-docker-app:latest
Open Docker Hub and verify that the image is uploaded.

Step 12: Pull Image from Docker Hub
docker pull klassy01/flask-docker-app:latest
Step 13: Run Pulled Image
Stop and remove the existing container:

docker stop flask-container
docker rm flask-container
Run the downloaded image:

docker run -d --name flask-test -p 5000:5000 klassy01/flask-docker-app:latest
Open:

http://localhost:5000
Step 14: Clean Up
docker stop flask-test
docker rm flask-test
