# Task 1: Setup and Configuration

## GitHub Repository Setup:
- Created a GitHub repository to host the source code.
- Uploaded the web application code to the repository.

Argo CD Installation:
- Followed the official Argo CD documentation to install and configure Argo CD on the Kubernetes cluster.
- Ensured proper connectivity and authentication between Argo CD and the Kubernetes cluster.

Argo Rollouts Installation:
- Installed the Argo Rollouts controller in the Kubernetes cluster as per the official guide.
- Verified the installation by checking the status of the Argo Rollouts controller.

# Task 2: Creating the GitOps Pipeline

Dockerizing the Application:
- Built a Docker image for the web application.
-Pushed the Docker image to a selected public container registry.

Deploying the Application Using Argo CD:
- Modified Kubernetes manifests in the GitHub repository to use the newly created Docker image.
- Configured Argo CD to monitor the GitHub repository for changes and automatically deploy updates to the Kubernetes cluster.

# Task 3: Implementing a Canary Release with Argo Rollouts

Rollout Strategy Definition:
- Modified the application's deployment to use Argo Rollouts, specifying a canary release strategy in the rollout definition.
- Defined the desired canary steps and metrics for monitoring.

Triggering a Rollout:
- Made a change to the application code, updated the Docker image, and pushed the new version to the container registry.
- Updated the rollout definition to use the new Docker image, triggering a canary release.

Monitoring the Rollout:
- Used Argo Rollouts to monitor the deployment of the new version.
- Monitored the canary release to ensure it successfully completed, considering metrics and user acceptance criteria.

Challenges and Resolutions:
Connectivity Issues:
- Encountered connectivity issues between Argo CD and the Kubernetes cluster.
- Ensured proper network configurations and resolved firewall issues to establish a connection.

Argo Rollouts Customization:
- Faced challenges in customizing the canary release strategy in Argo Rollouts.
- Consulted the Argo Rollouts documentation and community forums to understand the customization options and implemented the desired strategy.

Debugging Rollout Failures:
- Encountered issues during the canary release that led to failures.
- Utilized Argo Rollouts' debugging features, including analyzing logs and metrics, to identify and resolve issues in the rollout process.

- Overall, the implementation of the GitOps pipeline with Argo CD and Argo Rollouts proved successful after overcoming initial challenges related to connectivity and customization. The canary release strategy provided a controlled way to deploy changes, ensuring minimal impact on the production environment. Monitoring tools in Argo Rollouts facilitated the identification and resolution of issues during the canary release.

------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
Commands are Listed below
------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

- First I'm Download Kubernetes and It's Pre-requisite.
- Then Create EKS Cluster with ArgoCD.

## Installing Argo CD on Your Cluster
1.Check if kubectl is working as expected
  - kubectl get nodes
 
2.Create namespace
  - kubectl create namespace argocd
 
3.Run the Argo CD install script provided by the project maintainers.
  - kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
 
4. Check the status of Kubernetes pods
  - kubectl get pods -n argocd

# Forwarding Ports to Access Argo CD
5.1.Retrieve the admin password which was automatically generated during your installation and decode from base64 from online.
  - kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}"

5.2 forward those to arbitrarily chosen other ports, like 8080, like so:
  - kubectl port-forward svc/argocd-server -n argocd 8080:443
 
5.3 Access from ineternet from browser.
  - http://localhost:8080
    - user id : admin
    - password :base64 decoded password.

# Deploying an Example Application from argocd using argocd cli
7. Deploying an Application
  - argocd login localhost:8080
    - user id : admin
    - password :base64 decoded password.
  - argocd app create helm-guestbook --repo https://github.com/AjayPrakash7999/Argoapp.git --path helm-guestbook --dest-server https://kubernetes.default.svc --dest-namespace default
 
7.1 Check status inside argocd
  - argocd app get helm-guestbook

7.2 Actually deploy the application
  - argocd app sync helm-guestbook

-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
Screenshot Attached in Screenshot.md File in Git Repo



7.3 Port foward from onther session check the application status
  - kubectl port-forward svc/helm-guestbook 9090:80
  - http://localhost:9090
