# Kubernetes Practice Scenarios

This document outlines five integrated practice scenarios that help you master Kubernetes objects and concepts. Each scenario is designed to simulate a real-world deployment that brings together multiple Kubernetes objects, including Deployments, Services, ConfigMaps, Secrets, StatefulSets, Ingress, RBAC, Network Policies, Jobs, CronJobs, and Custom Resource Definitions (CRDs).

## Table of Contents
1. [Scenario 1: Full-Stack Web Application with Backend Database](#scenario-1-full-stack-web-application-with-backend-database)
2. [Scenario 2: Canary Deployment with Blue/Green Releases and Autoscaling](#scenario-2-canary-deployment-with-bluegreen-releases-and-autoscaling)
3. [Scenario 3: Multi-Tier Microservices Application with RBAC and Network Policies](#scenario-3-multi-tier-microservices-application-with-rbac-and-network-policies)
4. [Scenario 4: CI/CD Pipeline with Ephemeral Environments and Testing Jobs](#scenario-4-cicd-pipeline-with-ephemeral-environments-and-testing-jobs)
5. [Scenario 5: Custom Resource and Operator for Automated Application Backups](#scenario-5-custom-resource-and-operator-for-automated-application-backups)

---

## Scenario 1: Full-Stack Web Application with Backend Database

### Overview
Deploy a multi-tier web application consisting of:
- A **frontend** application.
- A **backend API**.
- A **database** managed with a StatefulSet.

This scenario integrates:
- **Deployments** for stateless services.
- **StatefulSet** for the database.
- **Services** for internal and external access.
- **ConfigMaps** and **Secrets** for configuration management.
- **PersistentVolumeClaims (PVCs)** for data persistence.
- **Ingress** for routing external traffic.

### Tasks
- **Configuration & Secrets:**
  - Create a `ConfigMap` for non-sensitive configuration (e.g., API endpoints, DB hostnames).
  - Create a `Secret` for storing sensitive data (e.g., database credentials).
- **Database Deployment:**
  - Deploy a `StatefulSet` for your database (e.g., PostgreSQL or MySQL) to provide stable network identities.
  - Define a `PersistentVolumeClaim (PVC)` for data persistence.
- **Backend API Deployment:**
  - Create a `Deployment` for the backend API that consumes the ConfigMap and Secret.
  - Expose the backend internally using a `Service` (ClusterIP).
- **Frontend Deployment:**
  - Deploy the frontend using a separate `Deployment` (e.g., NGINX or Node.js).
  - Expose the frontend with a `Service` (ClusterIP or NodePort).
- **Ingress Configuration:**
  - Configure an `Ingress` resource with an Ingress Controller (like NGINX) to route external HTTP/HTTPS traffic to the frontend service.

### Learning Outcomes
- Integrate configuration and secrets into your application.
- Manage stateful components with persistent storage.
- Understand traffic flow from external clients to internal services.
- Troubleshoot multi-tier connectivity issues.

---

## Scenario 2: Canary Deployment with Blue/Green Releases and Autoscaling

### Overview
Implement a progressive delivery strategy by deploying two versions of your application (blue and green) and gradually shifting traffic between them. This scenario incorporates:
- **Deployments** for both versions.
- **Services** for internal routing.
- **Ingress** for traffic splitting.
- **Horizontal Pod Autoscaler (HPA)** for dynamic scaling.
- **ConfigMaps** for managing routing rules.

### Tasks
- **Baseline Deployment:**
  - Create a `Deployment` for the stable (blue) version.
  - Expose the deployment via a `Service`.
- **Canary Deployment:**
  - Create a separate `Deployment` for the new (green) version with appropriate labeling.
- **Traffic Routing:**
  - Configure an `Ingress` resource with rules or annotations to distribute traffic between blue and green deployments.
  - Optionally, use a `ConfigMap` to manage the traffic split dynamically.
- **Autoscaling:**
  - Set up an `HPA` for both deployments based on CPU utilization or custom metrics.
  - Simulate load to validate autoscaling behavior.

### Learning Outcomes
- Implement and manage progressive delivery strategies.
- Utilize Ingress and ConfigMaps for dynamic traffic management.
- Configure autoscaling to handle varying loads.
- Coordinate between multiple deployments during application rollouts.

---

## Scenario 3: Multi-Tier Microservices Application with RBAC and Network Policies

### Overview
Build a microservices architecture composed of several interdependent services (e.g., user service, order service, and payment service). Secure inter-service communications with:
- **Deployments** and **Services** for each microservice.
- **ConfigMaps** for shared configuration.
- **RBAC** to manage access permissions.
- **Network Policies** to control pod-to-pod traffic.

### Tasks
- **Deploy Multiple Services:**
  - Create separate `Deployments` for each microservice.
  - Expose each microservice using its own `Service`.
- **Service Discovery & Configuration:**
  - Use a `ConfigMap` to store common configuration such as service endpoints.
- **Security with RBAC:**
  - Define `Roles` and `RoleBindings` (or `ClusterRoleBindings`) to limit access to Kubernetes resources.
  - Test the access restrictions by simulating different user roles.
- **Network Segmentation:**
  - Write `NetworkPolicy` manifests to restrict communication between services (e.g., allow only the user service to communicate with the order service).
  - Validate the network policies by testing connectivity between pods.

### Learning Outcomes
- Secure multi-service applications using RBAC and network policies.
- Manage inter-service communications in a distributed environment.
- Gain hands-on experience with writing and troubleshooting NetworkPolicy configurations.

---

## Scenario 4: CI/CD Pipeline with Ephemeral Environments and Testing Jobs

### Overview
Simulate a CI/CD pipeline by creating ephemeral test environments and running automated tests using Kubernetes Jobs and CronJobs. This scenario leverages:
- **Deployments** for temporary test environments.
- **Jobs** for running test suites.
- **CronJobs** for scheduling periodic tests.
- **ConfigMaps** and **Secrets** for environment configuration.
- **Ingress** for exposing test reporting interfaces.

### Tasks
- **Ephemeral Test Environment:**
  - Create a `Deployment` to spin up a temporary test environment based on the latest application code.
  - Use a `ConfigMap` and `Secret` to provide necessary configuration and credentials.
- **Automated Testing with Jobs:**
  - Define a `Job` that executes integration or unit tests against the test environment.
  - Ensure the job cleans up resources after completion.
- **Scheduled Testing:**
  - Set up a `CronJob` to run tests at regular intervals to catch regressions.
- **External Access for Test Reporting:**
  - Configure an `Ingress` resource to expose test reporting dashboards or endpoints.
  - Secure the reporting interface using RBAC.

### Learning Outcomes
- Automate provisioning and teardown of test environments.
- Integrate CI/CD pipelines with Kubernetes.
- Manage the lifecycle of batch jobs and scheduled tasks.
- Securely expose temporary testing and reporting interfaces.

---

## Scenario 5: Custom Resource and Operator for Automated Application Backups

### Overview
Extend Kubernetes by creating a Custom Resource Definition (CRD) and an operator to automate backups for a stateful application. This scenario integrates:
- **CRDs** for defining custom backup resources.
- **Operators** to automate backup processes.
- **Jobs** to perform backup operations.
- **PersistentVolumeClaims (PVCs)** for accessing application data.
- **Monitoring and Logging** tools for operational insight.
- **RBAC** for securing custom controller operations.

### Tasks
- **Define a Custom Resource:**
  - Create a `CRD` that defines a Backup resource including parameters like backup schedule, target PVC, and destination storage details.
- **Develop an Operator:**
  - Implement an operator (using Operator SDK or a custom controller) that watches for new Backup resources.
  - Trigger a `Job` from the operator to perform the backup when a new resource is created.
  - Ensure the job mounts the correct `PVC` and exports the backup data to the specified destination.
- **Monitoring and Logging:**
  - Integrate with monitoring tools (e.g., Prometheus) to track backup job performance and success.
  - Set up logging (e.g., with Fluentd) to capture detailed logs of the backup process.
- **Security & RBAC:**
  - Define appropriate RBAC rules to restrict the operator's access to only the necessary resources.
  - Secure any credentials or secrets used for external storage access.

### Learning Outcomes
- Extend Kubernetes functionality with custom resources and operators.
- Automate operational tasks like backups.
- Integrate persistent storage with automation.
- Enhance security and monitoring in operational workflows.

---

## Conclusion
These integrated scenarios are designed to simulate real-world deployments and help you develop a comprehensive understanding of how various Kubernetes objects interact in a production-like environment. By working through these exercises, you'll gain practical experience with:

- Multi-tier application deployment
- Progressive delivery and autoscaling strategies
- Security with RBAC and network policies
- CI/CD pipeline integration with ephemeral environments
- Extending Kubernetes with CRDs and operators

Happy learning and good luck with your journey to becoming a Kubernetes specialist!
