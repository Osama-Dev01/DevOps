Java Application Deployment on Kubernetes with Kyverno Policies
This project demonstrates the deployment of a scalable Java application on a Kubernetes cluster. It uses Kubernetes manifests and configurations to deploy the application, with additional features like automated policy enforcement through Kyverno.

Application Overview
The Java application in this project is deployed using a Kubernetes Deployment configuration. It includes the following components:

Deployment: A Kubernetes deployment ensures that the Java application is running in the cluster with the desired number of replicas.
Service: A Kubernetes service is used to expose the Java application within the cluster or externally.
Scaling: The deployment is configured to support scalability, ensuring the application can handle varying workloads.
The deployment YAML file (deployment.yaml) ensures that the pods are properly configured with labels and selectors, making it easier to manage and scale the application.

Adding Kyverno Policy to the Project
To enhance the governance and management of resources in the cluster, we added a Kyverno policy to enforce the presence of a specific label (app.kubernetes.io/name) on all pods. This helps ensure that resources adhere to a consistent naming convention, simplifying debugging, monitoring, and auditing.

Steps to Add the Policy
Follow these steps to add the Kyverno policy to the project:

1. Install Kyverno
Kyverno is a Kubernetes-native policy engine. Install it in the cluster using Helm or YAML:

bash
Copy code
helm repo add kyverno https://kyverno.github.io/kyverno/
helm repo update
helm install kyverno kyverno/kyverno --namespace kyverno --create-namespace
Verify the installation:

bash
Copy code
kubectl get pods -n kyverno
2. Create and Add the Policy
We created a Kyverno ClusterPolicy to enforce the app.kubernetes.io/name label on all pods. The policy file is located in kyverno-policies/pod-require-name-label.yaml.


To add the policy to the cluster:

bash
Copy code
kubectl apply -f kyverno-policies/pod-require-name-label.yaml
