# Adding Kyverno Policy to the Java app project

To enhance the governance and management of resources in the cluster, we added a Kyverno policy to enforce the presence of a specific label (`app.kubernetes.io/name`) on all pods. This helps ensure that resources adhere to a consistent naming convention, simplifying debugging, monitoring, and auditing.

## Steps to Add the Policy

## First clone the repo in local machine and run the K8s cluster

### 1. Install Kyverno
Kyverno is a Kubernetes-native policy engine. Install it in the cluster using Helm or YAML:


- helm repo add kyverno https://kyverno.github.io/kyverno/
- helm repo update
- helm install kyverno kyverno/kyverno --namespace kyverno --create-namespace```

#### Verify the installation:

- kubectl get pods -n kyverno

### 2. Create and Add the Policy
I have created a Kyverno ClusterPolicy to enforce the app.kubernetes.io/name label on all pods. The policy file is located in kyverno-policies/nano pod-require-name-label.yaml.

#### To add the policy to the cluster:

- kubectl apply -f kyverno-policies/nano pod-require-name-label.yaml

### 3. Update Deployment Manifests
To comply with the Kyverno policy, I updated the deployment manifest to include the required label for the pods. 

#### Apply the updated deployment:

- kubectl apply -f deployment.yaml

### 4. Test the Policy
To verify the policy enforcement:

- Deploy a pod without the **app.kubernetes.io/name** label.

- Observe that the pod creation is denied with an error similar to:

   - Error from server: admission webhook "validate.kyverno.svc" denied the request: label 'app.kubernetes.io/name' is required.
- Deploy resources with the required label. They will work seamlessly.

## Benefits of the Kyverno Policy
- **Consistency**: Ensures all pods are labeled appropriately, making resource management easier.
- **Auditing**: Simplifies auditing by enforcing a consistent labeling strategy.
- **Automation**: Automates compliance checks, reducing manual errors.

### How to View Policy Reports
Kyverno generates reports to monitor resource compliance. To view these reports:

- kubectl get policyreport
