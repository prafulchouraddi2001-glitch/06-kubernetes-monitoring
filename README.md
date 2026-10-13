# Kubernetes Monitoring and Operations Lab

A hands-on Kubernetes project focused on deploying containerized applications, managing workloads, implementing security and access controls, configuring storage, practising troubleshooting, and exploring monitoring with Prometheus and Grafana.

The project uses a local Kubernetes environment with Docker Desktop, WSL2, and a `kind` cluster. It is designed to build practical Kubernetes administration and DevOps skills through individual manifests and focused lab scenarios.

## Table of Contents

- [Project Overview](#project-overview)
- [Technologies](#technologies)
- [Learning Objectives](#learning-objectives)
- [Project Structure](#project-structure)
- [Prerequisites](#prerequisites)
- [Getting Started](#getting-started)
- [Kubernetes Labs](#kubernetes-labs)
- [Helm Chart](#helm-chart)
- [Monitoring](#monitoring)
- [Validation and Troubleshooting](#validation-and-troubleshooting)
- [Security Practices](#security-practices)
- [Cleanup](#cleanup)
- [Limitations](#limitations)
- [Author](#author)

## Project Overview

This project demonstrates hands-on practice with core Kubernetes concepts commonly used in DevOps and Cloud Engineering roles.

The repository contains Kubernetes YAML manifests and a Helm chart covering application deployment, services, autoscaling, networking, scheduling, storage, RBAC, security contexts, and common troubleshooting scenarios.

The exercises are intended to be applied individually, with the relevant prerequisites checked before each exercise.

## Technologies

| Technology | Purpose |
|---|---|
| Kubernetes | Container orchestration and workload management |
| kubectl | Cluster interaction and troubleshooting |
| kind | Local Kubernetes cluster |
| Docker Desktop | Local container and Kubernetes environment |
| Helm | Packaging and templating Kubernetes applications |
| YAML | Kubernetes resource definitions and configuration |
| Prometheus | Metrics collection and monitoring |
| Grafana | Metrics visualization and dashboards |
| Git and GitHub | Version control and project publication |

## Learning Objectives

- Deploy applications using Kubernetes Deployments and Services.
- Manage application configuration using ConfigMaps and Secrets.
- Understand horizontal scaling with the Horizontal Pod Autoscaler (HPA).
- Explore Kubernetes networking with ClusterIP, NodePort, LoadBalancer, and Ingress manifests.
- Apply node selectors, node affinity, and tolerations.
- Configure Role-Based Access Control (RBAC) using ServiceAccounts, Roles, and ClusterRoles.
- Practise workload security using Kubernetes SecurityContext settings.
- Explore persistent storage using PersistentVolumeClaims and StatefulSets.
- Package and render application resources using Helm.
- Diagnose common Pod and Service failures.
- Understand how Prometheus and Grafana support workload monitoring.

## Project Structure

```text
06-kubernetes-monitoring/
├── README.md
├── .gitignore
├── k8s/
│   ├── configmap.yaml
│   ├── deployment.yaml
│   ├── hpa.yaml
│   ├── ingress.yaml
│   ├── loadbalancer.yaml
│   ├── node-affinity.yaml
│   ├── node-affinity-failure.yaml
│   ├── node-affinity-notin.yaml
│   ├── node-affinity-preferred.yaml
│   ├── nodeport.yaml
│   ├── nodeselector.yaml
│   ├── nodeselector-failure.yaml
│   ├── rbac-cluster-pod-reader-binding.yaml
│   ├── rbac-cluster-pod-reader-role.yaml
│   ├── rbac-pod-reader-binding.yaml
│   ├── rbac-pod-reader-role.yaml
│   ├── rbac-serviceaccount.yaml
│   ├── securitycontext-basic.yaml
│   ├── service.yaml
│   ├── statefulset-service.yaml
│   ├── statefulset.yaml
│   ├── storage-pvc.yaml
│   ├── storage-test.yaml
│   ├── toleration-test.yaml
│   ├── troubleshooting-crashloop.yaml
│   ├── troubleshooting-imagepull.yaml
│   ├── troubleshooting-pending.yaml
│   └── troubleshooting-service.yaml
└── helm-webapp/
    ├── .helmignore
    ├── Chart.yaml
    ├── values.yaml
    └── templates/
        ├── _helpers.tpl
        ├── deployment.yaml
        └── service.yaml
```

**Note:** The local `k8s/secret.yaml` manifest is intentionally excluded from the public repository. Monitoring setup files and screenshots may be maintained separately from the manifests listed above.

## Prerequisites

Install or configure the following tools:

- Docker Desktop with WSL2 integration
- A running local Kubernetes cluster
- `kubectl`
- Helm
- Git

Check your current Kubernetes context and cluster nodes:

```bash
kubectl config current-context
kubectl get nodes
```

The development environment used for this project reported the context `kind-devops-k8s` and Kubernetes version `v1.34.0`. Your environment may differ.

## Getting Started

### 1. Clone the repository

After the project is published to GitHub:

```bash
git clone https://github.com/prafulchouraddi2001-glitch/06-kubernetes-monitoring.git
cd 06-kubernetes-monitoring
```

### 2. Verify cluster access

```bash
kubectl config current-context
kubectl get nodes
kubectl get namespaces
```

Make sure the active context points to the intended local cluster before applying manifests.

### 3. Review the manifests

Inspect the relevant YAML file before applying it:

```bash
cat k8s/deployment.yaml
cat k8s/service.yaml
```

Apply the basic application resources individually:

```bash
kubectl apply -f k8s/deployment.yaml
kubectl apply -f k8s/service.yaml
```

Check the resulting resources:

```bash
kubectl get deployments
kubectl get services
kubectl get pods
```

These commands assume the manifests match your active cluster and namespace configuration.

## Kubernetes Labs

### 1. Application Deployment and Configuration

Relevant files:

- `k8s/deployment.yaml`
- `k8s/service.yaml`
- `k8s/configmap.yaml`

Concepts practised:

- Kubernetes Deployment and Pod management
- Declarative resource configuration
- Service discovery and application access
- Separation of application configuration from workload definitions

### 2. Scaling

Relevant file:

- `k8s/hpa.yaml`

Concepts practised:

- Horizontal Pod Autoscaler configuration
- Replica scaling
- CPU-based scaling behaviour
- Observing scale-up and scale-down behaviour

HPA operation depends on a compatible metrics API, such as Metrics Server, and correctly configured resource requests. Review these prerequisites before testing autoscaling.

### 3. Networking

Relevant files:

- `k8s/service.yaml`
- `k8s/nodeport.yaml`
- `k8s/loadbalancer.yaml`
- `k8s/ingress.yaml`

Concepts practised:

- ClusterIP Services
- NodePort Services
- LoadBalancer Service configuration
- Ingress resource configuration
- Service selectors and application traffic routing

An Ingress resource does not automatically provide an external endpoint. An appropriate Ingress controller must be installed and configured. LoadBalancer behaviour also depends on the cluster environment and available integrations.

### 4. Scheduling

Relevant files:

- `k8s/nodeselector.yaml`
- `k8s/nodeselector-failure.yaml`
- `k8s/node-affinity.yaml`
- `k8s/node-affinity-notin.yaml`
- `k8s/node-affinity-preferred.yaml`
- `k8s/node-affinity-failure.yaml`
- `k8s/toleration-test.yaml`

Concepts practised:

- Node selectors
- Required and preferred node affinity
- Scheduling constraints
- Tolerations and taints
- Diagnosing Pods that remain Pending because of scheduling requirements

Review the node labels, taints, and resource capacity before running these scenarios.

### 5. RBAC and ServiceAccounts

Relevant files:

- `k8s/rbac-serviceaccount.yaml`
- `k8s/rbac-pod-reader-role.yaml`
- `k8s/rbac-pod-reader-binding.yaml`
- `k8s/rbac-cluster-pod-reader-role.yaml`
- `k8s/rbac-cluster-pod-reader-binding.yaml`

Concepts practised:

- ServiceAccounts
- Namespaced Roles and RoleBindings
- ClusterRoles and ClusterRoleBindings
- Kubernetes authorization
- Restricting access to specific resources and actions

RBAC manifests can grant meaningful permissions. Review their rules and binding subjects before applying them, especially in shared clusters.

### 6. Security Contexts

Relevant file:

- `k8s/securitycontext-basic.yaml`

Concepts practised:

- Container and Pod security settings
- Running workloads with restricted privileges
- Understanding filesystem and privilege restrictions

Review the actual manifest to determine which security controls it enables. Additional controls may be required for a production workload.

### 7. Storage

Relevant files:

- `k8s/storage-pvc.yaml`
- `k8s/storage-test.yaml`
- `k8s/statefulset.yaml`
- `k8s/statefulset-service.yaml`

Concepts practised:

- PersistentVolumeClaims
- Persistent storage for workloads
- StatefulSet fundamentals
- Stable workload identities and storage considerations

Storage provisioning depends on the cluster's available StorageClasses and provisioners. Check these before creating claims:

```bash
kubectl get storageclass
kubectl get pv
kubectl get pvc --all-namespaces
```

### 8. Troubleshooting

Relevant files:

- `k8s/troubleshooting-pending.yaml`
- `k8s/troubleshooting-crashloop.yaml`
- `k8s/troubleshooting-imagepull.yaml`
- `k8s/troubleshooting-service.yaml`

The lab scenarios cover common Kubernetes symptoms, including:

- `Pending`
- `CrashLoopBackOff`
- `ImagePullBackOff`
- Service selector or connectivity problems

Useful diagnostic commands:

```bash
kubectl get pods -A
kubectl describe pod <pod-name>
kubectl logs <pod-name>
kubectl get events --sort-by=.metadata.creationTimestamp
kubectl get services
kubectl get endpointslices
```

Replace placeholders such as `<pod-name>` with the actual resource name. Use the appropriate namespace when necessary.

## Helm Chart

The `helm-webapp/` directory contains a Helm chart with:

- `Chart.yaml` — chart metadata
- `values.yaml` — configurable values
- `templates/deployment.yaml` — Deployment template
- `templates/service.yaml` — Service template
- `templates/_helpers.tpl` — reusable template helpers

### Validate the chart

Lint the chart:

```bash
helm lint ./helm-webapp
```

Render the Kubernetes resources locally without installing them:

```bash
helm template webapp-release ./helm-webapp
```

Review the rendered YAML before installing or upgrading a release.

### Install the chart (optional lab)

After reviewing the chart and confirming the active cluster:

```bash
helm install webapp-release ./helm-webapp
```

Check the release and its resources:

```bash
helm list
kubectl get deployments
kubectl get services
kubectl get pods
```

If the release already exists, inspect it before deciding whether to use `helm upgrade` or another action. Avoid creating duplicate resources or applying overlapping manifests without understanding their ownership.

The chart's successful linting and rendering validate its structure; they do not, by themselves, prove that it has been installed successfully.

## Monitoring

Prometheus and Grafana were used in the local Kubernetes learning environment to explore cluster and workload metrics.

Monitoring concepts included:

- CPU utilisation
- Memory utilisation
- Pod and node metrics
- Dashboard-based observation of workload behaviour
- Using metrics to understand scaling and troubleshooting

The Prometheus/Grafana deployment configuration and screenshots are not included in the repository structure listed above. This section documents the monitoring work performed in the local lab rather than claiming that the monitoring stack can be installed directly from this repository.

## Validation and Troubleshooting

Before applying a manifest, review it and check that the resource names, namespaces, image references, and dependencies match your environment.

Useful commands:

```bash
kubectl get nodes
kubectl get pods -A
kubectl get deployments -A
kubectl get services -A
kubectl get events -A --sort-by=.metadata.creationTimestamp
helm lint ./helm-webapp
helm template webapp-release ./helm-webapp
```

For a Pod that is not running, begin with `kubectl describe pod` and inspect the events. For application failures, inspect container logs. For connectivity issues, check Service selectors, endpoints, namespaces, and the relevant networking configuration.

### Prometheus Target Health and Troubleshooting

Prometheus was configured to monitor Kubernetes components and supporting services in the local `kind` cluster.

**Validation results**
- 14 active targets reported `UP`.
- 4 active targets reported `DOWN`.
- The unavailable targets were kube-controller-manager, kube-scheduler, kube-etcd, and kube-proxy.
- The first two components were configured to bind their metrics endpoints to `127.0.0.1`; etcd's metrics listener was also bound to localhost.
- The kube-proxy target returned a connection-refused error, while its `metricsBindAddress` configuration was empty.

**Root cause:** Prometheus attempted to scrape metrics through the node's container IP, but the corresponding endpoints were not accepting connections at those addresses.

**Security consideration:** Control-plane metrics endpoints were not exposed more broadly just to achieve an all-green target dashboard. This remains a documented limitation of the local monitoring environment.


## Security Practices

- Do not commit real passwords, API keys, access tokens, or cloud credentials.
- The local `k8s/secret.yaml` file is excluded from version control.
- Do not use placeholder Secret values as a substitute for secure secret management.
- Review RBAC permissions before applying RoleBindings or ClusterRoleBindings.
- Use least-privilege permissions for workloads and users.
- Review SecurityContext settings and container images before using them outside a learning environment.
- Avoid applying every manifest together: some files intentionally demonstrate failure cases or alternative configurations.
- Treat local configuration and credentials as environment-specific.

For real deployments, use an appropriate secrets-management approach and follow your organisation's security requirements.

## Cleanup

Before removing resources, inspect what exists and identify which resources belong to this lab.

For a Helm release installed during the exercise:

```bash
helm list
helm uninstall webapp-release
```

For individual manifests, remove only the resources that you intentionally created and have confirmed are safe to delete.

**Warning:** deleting PVCs, PVs, StatefulSets, or other resources may cause data loss. Do not use broad cleanup commands in a shared cluster or when resource ownership is unclear.

## Limitations

- This is a local Kubernetes learning lab, not a production-ready platform.
- Networking behaviour depends on the local cluster and its installed controllers.
- HPA requires a working metrics API and suitable workload resource configuration.
- Storage behaviour depends on available provisioners and StorageClasses.
- Monitoring setup files and dashboard screenshots are not included in the repository structure described here.
- Amazon EKS deployment is planned as a separate project and is not claimed as part of this local implementation.

## Author

**Praful Chouraddi**

MCA Graduate | Aspiring DevOps / Cloud Engineer

GitHub: [prafulchouraddi2001-glitch](https://github.com/prafulchouraddi2001-glitch)

This project represents hands-on learning with Kubernetes, Helm, workload operations, access control, storage, security, troubleshooting, and local monitoring.
