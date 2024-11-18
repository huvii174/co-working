Coworking Space Service Extension

The Coworking Space Service provides APIs that enable users to request one-time tokens and administrators to authorize coworking space access. Built with a microservice architecture, it allows independent deployment and management of its components.

Getting Started

Prerequisites:

Local Environment:

1.  Python 3.6+ for running applications and managing dependencies with pip.
    
2.  Docker CLI for building and running Docker images locally.
    
3.  kubectl for managing Kubernetes clusters.
    
4.  Helm for deploying Helm Charts to Kubernetes clusters.
    

Remote Resources:

1.  AWS CodeBuild for building Docker images remotely.
    
2.  AWS ECR for hosting Docker images.
    
3.  AWS EKS (Kubernetes) for running applications.
    
4.  AWS CloudWatch for monitoring logs and metrics in Kubernetes.
    
5.  AWS CodePipeline for CI/CD integration.

5.  GitHub for version control.
    

Setup Instructions

1.  Database Setup with PostgreSQL:
    

Pre-requisites:

*   A Kubernetes cluster is ready for use.
    
*   kubectl and Helm are installed and configured.
    

Steps:

*   Install Helm:curl -LO [https://get.helm.sh/helm-v3.12.2-linux-amd64.tar.gz](https://get.helm.sh/helm-v3.12.2-linux-amd64.tar.gz)
    
*   Add the Bitnami Helm Repository and update:helm repo add bitnami [https://charts.bitnami.com/bitnami](https://charts.bitnami.com/bitnami)helm repo update
    
*   Install the PostgreSQL Chart:helm install my-postgres bitnami/postgresql
    
*   Verify installation:helm listkubectl get pods
    
*   Retrieve PostgreSQL credentials:export POSTGRES\_PASSWORD=$(kubectl get secret --namespace default my-postgres-postgresql -o jsonpath="{.data.postgres-password}" | base64 --decode)
    

1.  Running the Analytics Application Locally:
    

Steps:

*   Install dependencies:apt updateapt install build-essential libpq-devpip install --upgrade pip setuptools wheelpip install -r requirements.txt
    
*   Run the application using environment variables:DB\_USERNAME=username\_here DB\_PASSWORD=password\_here python app.py
    

Required environment variables:DB\_USERNAME: Database usernameDB\_PASSWORD: Database passwordDB\_HOST: Defaults to 127.0.0.1DB\_PORT: Defaults to 5432DB\_NAME: Defaults to postgres

*   Verify the application:
    
    *   Generate daily usage report:curl /api/reports/daily\_usage
        
    *   Generate user visit report:curl /api/reports/user\_visits
        

Kubernetes Deployment

Steps:

1.  Navigate to the deployment directory:cd deployment
    
2.  Apply configurations:
- kubectl apply -f pvc.yml
- kubectl apply -f pv.yml
- kubectl apply -f postgresql-deployment.yml
- kubectl apply -f postgresql-service.yml
- kubectl apply -f ConfigMap.yml
- kubectl apply -f secrets.yml
- kubectl apply -f deployment.yml
    

Deployment Process

Overview:The deployment process uses Kubernetes and Helm for scalability. AWS EKS, CodeBuild, and ECR streamline the CI/CD pipeline. AWS CodePipeline integrates with GitHub to automate updates.

Workflow for Deploying Updates:

*   Make code changes and commit them to GitHub.
    
*   CodePipeline automatically builds the changes using CodeBuild.
    
*   CodePipeline pushes built Docker images to AWS ECR.
    
*   Update Kubernetes manifests and apply changes using kubectl.
    

Additional Recommendations:

1.  Memory and CPU Allocation:Set resource limits in Kubernetes configuration to prevent over-provisioning, such as 200m CPU and 512Mi memory for efficient resource use.
    
2.  AWS Instance Type:Use t3.medium for a balance between cost and performance. It provides adequate resources for moderate workloads.
    
3.  Cost Optimization:
    
    *   Use spot instances for non-critical workloads in EKS.
        
    *   Monitor resource usage with CloudWatch to eliminate underutilization.
        
    *   Employ auto-scaling to adjust resource usage based on demand.