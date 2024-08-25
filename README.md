# my-simple-app

1. Repository Structure
```
my-simple-app/
├── .github/
│   └── workflows/
│       └── ci-cd.yml
├── Dockerfile
├── app.py
└── README.md

```
2. app.py (a simple Python web application using Flask)
```
from flask import Flask

app = Flask(__name__)

@app.route('/')
def hello():
    return "Welcome to cloudera Education!"

if __name__ == '__main__':
    app.run(host='0.0.0.0', port=8080)

```
3. Dockerfile (Dockerfile to containerize the Flask app)
```
# Use an official Python runtime as a parent image
FROM python:3.9-slim

# Set the working directory in the container
WORKDIR /app

# Copy the current directory contents into the container at /app
COPY . /app

# Install any needed packages specified in requirements.txt
RUN pip install Flask

# Make port 8080 available to the world outside this container
EXPOSE 8080

# Define environment variable
ENV NAME World

# Run app.py when the container launches
CMD ["python", "app.py"]

```
4. GitHub Actions Workflow: ci-cd.yml
```
name: CI/CD Pipeline

on:
  push:
    branches:
      - main
  pull_request:
    branches:
      - main

jobs:
  build:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout code
        uses: actions/checkout@v3

      - name: Set up Docker Buildx
        uses: docker/setup-buildx-action@v3

      - name: Build Docker image
        run: docker build -t my-simple-app .

      - name: Run Docker container
        run: docker run -d -p 8080:8080 my-simple-app

      - name: Wait for the application to start
        run: sleep 10

      - name: Test containerized application
        run: curl -v http://localhost:8080
```
Explanation

    Project Structure: The project consists of a simple Python web application (app.py), a Dockerfile for containerizing the app, and a GitHub Actions workflow (ci-cd.yml).

    GitHub Actions Workflow:
        Checkout Code: Pulls the code from the GitHub repository.
        Set up Docker Buildx: Prepares the environment for building Docker images.
        Build Docker Image: Builds the Docker image locally without pushing it to any Docker registry.
        Run Docker Container: Runs the containerized application.
        Test Containerized Application: Sends an HTTP request to the application running inside the container to verify it's working.

Steps to Implement

    1. Create a new GitHub repository with the structure outlined above.
    2. Push the code to your repository.
    3. Create a GitHub Actions Workflow:
        i. Navigate to the Actions tab in your GitHub repository.
        ii. Create a new workflow and paste the contents of ci-cd.yml.
    4. Push a commit or create a pull request to trigger the workflow.
    5. Monitor the workflow in the Actions tab. It will build the Docker image, run the container, and test the application.

This setup allows students to learn how to use GitHub Actions for CI/CD without requiring a Docker registry. They can see the entire process from building the image to running and testing it within the GitHub Actions environment.
