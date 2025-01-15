# Helm Charts Repository

## Repository Name: `helm-charts`

### Purpose
This repository provides a collection of reusable and well-documented Helm charts to simplify the deployment and management of Kubernetes applications. It includes templates for stateless, stateful, and complex system deployments, serving as a valuable resource for DevOps engineers and developers alike.

---

## Table of Contents
1. [Repository Structure](#repository-structure)
2. [Getting Started](#getting-started)
3. [Helm Chart Guidelines](#helm-chart-guidelines)
4. [Chart Details](#chart-details)
5. [Best Practices](#best-practices)
6. [Contributing](#contributing)
7. [License](#license)

---

## Repository Structure

The repository is organized as follows:

```
helm-charts/
├── charts/
│   ├── stateless-app/
│   │   ├── templates/
│   │   ├── values.yaml
│   │   └── Chart.yaml
│   ├── stateful-app/
│   │   ├── templates/
│   │   ├── values.yaml
│   │   └── Chart.yaml
│   └── complex-system/
│       ├── templates/
│       ├── values.yaml
│       └── Chart.yaml
└── README.md
```

### Directory Descriptions
- **charts/**: Contains subdirectories for different types of Helm charts.
  - **stateless-app/**: Helm chart template for deploying stateless applications.
  - **stateful-app/**: Helm chart template for deploying stateful applications.
  - **complex-system/**: Helm chart template for deploying complex, multi-service systems.

Each chart directory includes the following files:
- **templates/**: YAML templates for Kubernetes resources.
- **values.yaml**: Default configuration values for the chart.
- **Chart.yaml**: Metadata defining the chart.

---

## Getting Started

### Prerequisites
1. [Kubernetes Cluster](https://kubernetes.io/docs/setup/) (v1.18+ recommended)
2. [Helm CLI](https://helm.sh/docs/intro/install/) (v3.x recommended)

### Installation
1. Clone this repository:
   ```bash
   git clone https://github.com/your-organization/helm-charts.git
   cd helm-charts
   ```
2. Navigate to the desired chart directory (e.g., `stateless-app`).
3. Deploy the chart using:
   ```bash
   helm install my-release ./chart-directory
   ```
   Replace `my-release` with your release name and `./chart-directory` with the desired chart directory path.

---

## Helm Chart Guidelines

### Chart Directory Structure
Ensure each chart directory follows this structure:

```
chart-directory/
├── Chart.yaml
├── values.yaml
└── templates/
    └── [Kubernetes resource YAML files]
```

### Example Chart.yaml (for Stateless App)
```yaml
apiVersion: v2
name: stateless-app
version: 0.1.0
appVersion: 1.0.0
description: A Helm chart for deploying stateless applications
```

### Naming Conventions
- Use descriptive chart names.
- Follow Kubernetes resource naming conventions.

### Chart Versioning
- Adhere to [Semantic Versioning](https://semver.org/).
- Update the chart version in `Chart.yaml` with each change.

---

## Chart Details

### Stateless App Chart
- Deploys scalable stateless services.
- Supports configuration for replicas, service ports, and resource limits.

#### Example values.yaml (for Stateless App)
```yaml
replicaCount: 3
image:
  repository: nginx
  tag: stable
  pullPolicy: IfNotPresent
service:
  type: ClusterIP
  port: 80
resources:
  limits:
    cpu: 500m
    memory: 512Mi
```

#### Example Deployment YAML (Stateless App)
```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
spec:
  replicas: {{ .Values.replicaCount }}
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: {{ .Values.image.repository }}:{{ .Values.image.tag }}
        ports:
        - containerPort: {{ .Values.service.port }}
```

### Stateful App Chart
- Deploys applications requiring persistent storage and stable network identities.
- Includes StatefulSets and PersistentVolumeClaims.

#### Example values.yaml (for Stateful App)
```yaml
replicaCount: 2
volumeClaimTemplates:
  - metadata:
      name: data
    spec:
      accessModes: ["ReadWriteOnce"]
      resources:
        requests:
          storage: 1Gi
```

#### Example StatefulSet YAML
```yaml
apiVersion: apps/v1
kind: StatefulSet
metadata:
  name: stateful-app
spec:
  serviceName: "stateful-app"
  replicas: {{ .Values.replicaCount }}
  selector:
    matchLabels:
      app: stateful-app
  template:
    metadata:
      labels:
        app: stateful-app
    spec:
      containers:
      - name: app
        image: {{ .Values.image.repository }}:{{ .Values.image.tag }}
        volumeMounts:
        - name: data
          mountPath: /data
  volumeClaimTemplates:
  - metadata:
      name: data
    spec:
      accessModes: ["ReadWriteOnce"]
      resources:
        requests:
          storage: 1Gi
```

### Complex System Chart
- Manages multi-service deployments with interdependent components.
- Supports configuration for multiple sub-charts and dependencies.

#### Example values.yaml (for Complex System)
```yaml
components:
  backend:
    image: backend-app:latest
    replicaCount: 2
  frontend:
    image: frontend-app:stable
    replicaCount: 1
```

#### Example Kubernetes Service YAML
```yaml
apiVersion: v1
kind: Service
metadata:
  name: frontend-service
spec:
  selector:
    app: frontend
  ports:
  - protocol: TCP
    port: 80
    targetPort: 8080
  type: LoadBalancer
```

---

## Best Practices
1. Use values.yaml for default configurations.
2. Document customizations in the chart-specific README.
3. Maintain backward compatibility for existing configurations.

---

## Contributing
We welcome contributions to improve and expand the available Helm charts.

### Steps to Contribute
1. Fork the repository.
2. Create a new branch for your feature or bug fix.
3. Ensure all new code follows best practices and passes lint checks.
4. Submit a pull request with a detailed description of your changes.

---

