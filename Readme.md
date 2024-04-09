# GitHub Actions DevSecOps Pipeline with Kubernetes and ArgoCD
## Pre-Requisites
Ensure the following prerequisites are set up before executing the pipeline:

### 1. SonarQube
Install SonarQube on a server and configure it for use within the pipeline. Generate a global or project-specific Sonar token. Create a file named ```sonar-project.properties``` in your project directory and insert the following content:


```shell
sonar.projectKey=<Project-Key>
sonar.projectName=<Project-Name>

# Source code directories
sonar.sources=.

# Language of the source code
sonar.language=javascript

# Encoding of the source code
sonar.sourceEncoding=UTF-8

# Exclusions if needed
sonar.exclusions=**/node_modules/**, **/out/**, **/build/**
``` 

💡 Customize this file based on your requirements.

### 2. SSH Configuration for Manifests Repository
Generate an SSH key pair and add the public key to the deploy keys section of the manifests repository. Save the private key for creating a secret later.

### 3. NTFY Configuration
Set up NTFY on a separate server and create a dedicated user and topic. Generate a token for this user and retain it for later use.

### 4. Kubernetes Cluster with ArgoCD
Ensure a functional Kubernetes installation with ArgoCD configured. Also, define an ingress and the corresponding routes for your application.

### 5. Repository Secrets
Navigate to ```Settings -> Secrets and Variables -> Actions``` and create the following secrets:

APP_URL: The Ingress URL on Kubernetes where your application will be accessible. This is required for OWASP ZAP to conduct a DAST Scan.

DOCKERHUB_TOKEN: Your DockerHub password for image pushing.

DOCKERHUB_USERNAME: Your DockerHub username.

NTFY_HEADER: The header you wish to include in NTFY notifications.

NTFY_TOPIC: The NTFY topic for sending notifications.

SONAR_HOST_URL: The URL to access SonarQube.

SONAR_TOKEN: The Sonar token generated earlier.

SSH_PRIVATE_KEY: The private key for cloning the private GitHub repo containing your manifest files.

## Trivy and OWASP Reports
Trivy scan results will be available in the Security tab of the repository. As for the DAST reports, you can either download them from the artifacts generated post-pipeline execution or view them as issues in the Issues tab.