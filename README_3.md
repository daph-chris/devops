Exercise 3 – CI Pipeline for a Python App Using GitHub Actions and Docker Hub
We'll use GitHub Actions to automatically test a Python application, build a Docker image, and push it to Docker Hub.

Project folder: python-ci-docker-lab

Step 1: Create project folder
Open PowerShell:

cd Desktop
mkdir python-ci-docker-lab
cd python-ci-docker-lab
code .
Step 2: Create app.py
notepad app.py
Paste:

def add(a, b):    return a + bif __name__ == "__main__":    print("Hello from Python CI Lab!")    print("2 + 3 =", add(2, 3))
Save and close.

Step 3: Create test file
Create the tests folder:

mkdir tests
notepad tests/test_app.py
Paste:

from app import adddef test_add_positive_numbers():    assert add(2, 3) == 5def test_add_negative_numbers():    assert add(-1, 1) == 0
Save and close.

Step 4: Create requirements.txt
notepad requirements.txt
Paste:

pytest==8.3.2
Save and close.

Step 5: Create Dockerfile
notepad Dockerfile
Paste:


FROM python:3.11-slim

WORKDIR /app

COPY app.py .

CMD ["python", "app.py"]
Save and close.

Step 6: Test the Python application locally
Install dependencies:

python -m pip install -r requirements.txt
Run the application:

python app.py
Expected output:

Hello from Python CI Lab!
2 + 3 = 5
Step 7: Run Python tests
pytest -q
Expected output:

2 passed
Step 8: Create GitHub Actions workflow folder
Run:

mkdir .github
mkdir .github\workflows
notepad .github\workflows\ci-dockerhub.yml
Paste this complete workflow:


name: Python CI and Docker Hub

on:
  push:
    branches: [main]
  pull_request:
    branches: [main]

jobs:
  build:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout code
        uses: actions/checkout@v4

      - name: Set up Python
        uses: actions/setup-python@v5
        with:
          python-version: '3.11'

      - name: Install dependencies
        run: pip install -r requirements.txt

      - name: Run tests
        run: pytest -q

      - name: Build Docker image
        if: github.event_name == 'push'
        run: docker build -t ${{ secrets.DOCKERHUB_USERNAME }}/python-ci-docker-lab:latest .

      - name: Login to Docker Hub
        if: github.event_name == 'push'
        uses: docker/login-action@v3
        with:
          username: ${{ secrets.DOCKERHUB_USERNAME }}
          password: ${{ secrets.DOCKERHUB_TOKEN }}

      - name: Push Docker image
        if: github.event_name == 'push'
        run: docker push ${{ secrets.DOCKERHUB_USERNAME }}/python-ci-docker-lab:latest
Save and close.

Step 9: Create GitHub repository
Open GitHub.

Click New repository.

Repository name: python-ci-docker-lab.

Select Public.

Click Create repository.

Step 10: Add Docker Hub credentials to GitHub
First, open Docker Hub.

Sign in using your Docker Hub account.

Create a repository named python-ci-docker-lab.

Generate an access token from Docker Hub account settings.

Now open your GitHub repository:

Go to Settings.

Select Secrets and variables → Actions.

Click New repository secret.

Create these two secrets:

Secret name

Value

DOCKERHUB_USERNAME

klassy01

DOCKERHUB_TOKEN

Your Docker Hub access token

Save both secrets.

Step 11: Push project to GitHub
Return to PowerShell:

git init
git branch -M main

git add .
git commit -m "Initial commit"

git remote add origin https://github.com/Daph-Chris/python-ci-docker-lab.git

git push -u origin main
Step 12: Check GitHub Actions
Open your GitHub repository.

Click the Actions tab.

You should see Python CI and Docker Hub.

Click the workflow.

Open the latest workflow run.

Check that all steps have green ticks.


Expected Result
The Python tests should pass, Docker image should build, and the image should be pushed to Docker Hub.
Step 13: Verify Docker Hub
Open Docker Hub and go to your repository:

klassy01/python-ci-docker-lab

You should see the latest image tag.

Step 14: Optional – Create Version Tag
git tag v1.0.0
git push origin v1.0.0
This creates and pushes the version tag.

python-ci-docker-lab/
│
├── app.py
├── requirements.txt
├── Dockerfile
│
├── tests/
│   └── test_app.py
│
└── .github/
    └── workflows/
        └── ci-dockerhub.yml

