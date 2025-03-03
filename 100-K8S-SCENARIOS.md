# 100 Kubernetes Scenarios

## Scenario 1: Automating a Multi-Region Kubernetes Cluster Deployment
**Original Task:** Introduction to Kubernetes  
**Theory:** Dive into Kubernetes architecture—control plane (API server, etcd, scheduler), worker nodes (kubelet, kube-proxy), and multi-cluster considerations like federation and disaster recovery.  
**Use Case:** Automate the provisioning of an AWS EKS cluster across two regions for high availability.

**Hands-On:**
- Use Terraform to define an EKS cluster with multi-AZ node groups, VPC peering, and an S3 backend for state.
- Configure eksctl as a fallback automation tool and deploy the cluster.
- Validate cluster health with `kubectl get nodes --all-namespaces`.

**Mini-Project:**
- Deploy an NGINX Pod using a Kubernetes manifest stored in a Git repository.
- Automate deployment with FluxCD to sync the manifest to EKS.
- Verify NGINX is running with Datadog metrics integration for node health.

**Integration:** AWS (EKS, VPC, S3), Terraform, FluxCD, Datadog.  
**Automation:** Terraform scripts and FluxCD GitOps pipeline reduce manual steps.

---

## Scenario 2: Real-Time Log Aggregation with Multi-Container Pods
**Original Task:** Pods  
**Theory:** Explore Pods’ lifecycle, multi-container patterns (sidecar, ambassador), and Kubernetes scheduler placement logic.  
**Use Case:** Build a logging pipeline for a microservices app where each Pod aggregates logs to AWS CloudWatch.

**Hands-On:**
- Create a YAML for a Pod with an NGINX container and a Fluentd sidecar.
- Configure Fluentd to forward logs to CloudWatch Logs using AWS credentials mounted as a Kubernetes Secret.
- Apply the Pod to EKS with `kubectl apply -f pod.yaml`.

**Mini-Project:**
- Simulate NGINX traffic with a Kubernetes Job that sends HTTP requests.
- Automate log forwarding configuration via a Helm chart deployed through FluxCD.
- Verify logs in CloudWatch and set up a Datadog log pipeline to parse them.

**Integration:** AWS (EKS, CloudWatch), Kubernetes, Fluentd, Helm, FluxCD, Datadog.  
**Automation:** Helm and FluxCD streamline deployment and updates of the logging setup.

---

## Scenario 3: Organizing a Multi-Tenant Kubernetes Environment
**Original Task:** Labels and Selectors  
**Theory:** Master labels, selectors, annotations, and their roles in RBAC, NetworkPolicies, and resource filtering.  
**Use Case:** Set up a multi-tenant EKS cluster where teams are isolated by labels and selectors.

**Hands-On:**
- Tag Pods with labels (e.g., `team=frontend`, `env=prod`) across multiple namespaces.
- Use `kubectl get pods -l team=frontend` to filter resources dynamically.
- Define a NetworkPolicy to allow traffic only between Pods with matching labels.

**Mini-Project:**
- Terraform an EKS cluster with namespaces for team-a and team-b.
- Deploy a Spring Boot app with team-specific labels, synced via FluxCD.
- Verify isolation with Datadog dashboards showing team-specific metrics.

**Integration:** AWS (EKS), Kubernetes, Terraform, FluxCD, Datadog, Spring Boot.  
**Automation:** Terraform provisions namespaces; FluxCD applies labeled manifests.

---

## Scenario 4: Scalable Microservices Deployment with Zero-Downtime Updates
**Original Task:** Deployments  
**Theory:** Study Deployments’ replica management, rolling updates, rollback strategies, and HorizontalPodAutoscaler (HPA).  
**Use Case:** Deploy a scalable Spring Boot microservice on EKS with automated scaling and updates.

**Hands-On:**
- Create a Deployment YAML for a Spring Boot app with 3 replicas using a RollingUpdate strategy.
- Integrate with an AWS ALB via Kubernetes ingress.
- Scale manually with `kubectl scale deployment --replicas=5`.

**Mini-Project:**
- Automate deployment with FluxCD and a Git repo containing the YAML.
- Set up HPA to scale based on CPU/memory metrics collected by Datadog.
- Perform a zero-downtime update by changing the Spring Boot image tag and verify with `kubectl rollout status`.

**Integration:** AWS (EKS, ALB), Kubernetes, Spring Boot, FluxCD, Datadog.  
**Automation:** FluxCD handles updates; HPA auto-scales based on Datadog metrics.

---

## Scenario 5: Service Mesh Integration for Microservices Communication
**Original Task:** Services  
**Theory:** Explore Service types (ClusterIP, NodePort, LoadBalancer), service discovery, DNS, and integration with service meshes like Istio.  
**Use Case:** Expose a Spring Boot microservice cluster with Istio on EKS, integrating with AWS ALB.

**Hands-On:**
- Deploy a Spring Boot app with a ClusterIP Service and an Istio sidecar.
- Configure an Istio VirtualService for routing and traffic splitting.
- Test connectivity with `kubectl exec` from another Pod.

**Mini-Project:**
- Terraform an EKS cluster with Istio installed via Helm and FluxCD.
- Set up an AWS ALB ingress to route external traffic to the Service.
- Monitor service metrics and traces in Datadog APM integrated with Istio.

**Integration:** AWS (EKS, ALB), Kubernetes, Spring Boot, Istio, Terraform, FluxCD, Datadog.  
**Automation:** Terraform and FluxCD automate cluster and Istio setup; Istio handles traffic management.

---

## Scenario 6: Dynamic Configuration Management for a Kafka Consumer
**Original Task:** ConfigMaps  
**Theory:** Dive into ConfigMaps’ use cases, volume mounts, environment variables, live updates, and ConfigMap rollouts.  
**Use Case:** Configure a Spring Boot Kafka consumer running on EKS with dynamic settings.

**Hands-On:**
- Create a ConfigMap with Kafka broker URLs and topic names.
- Mount it as a volume in a Spring Boot Deployment and restart to apply changes.
- Update the ConfigMap with `kubectl edit configmap` and observe behavior.

**Mini-Project:**
- Automate ConfigMap creation via Terraform and sync with FluxCD.
- Integrate with AWS MSK as the Kafka backend.
- Use Datadog to monitor consumer lag and configuration effects.

**Integration:** AWS (EKS, MSK), Kubernetes, Spring Boot, Kafka, Terraform, FluxCD, Datadog.  
**Automation:** Terraform provisions MSK; FluxCD syncs ConfigMaps dynamically.

---

## Scenario 7: Secure Credential Management for an Angular Frontend
**Original Task:** Secrets  
**Theory:** Study Secrets’ encryption, base64 encoding, Kubernetes Secrets vs. external vaults, and secret rotation strategies.  
**Use Case:** Securely manage API keys for an Angular app running on EKS, integrating with AWS Secrets Manager.

**Hands-On:**
- Create a Secret with an API key using `kubectl create secret generic`.
- Mount it as an environment variable in an Angular Pod.
- Rotate the secret manually and verify app behavior.

**Mini-Project:**
- Use Terraform to integrate AWS Secrets Manager with EKS IRSA (IAM Roles for Service Accounts).
- Automate Secret deployment with FluxCD and a CSI Secrets Store Provider.
- Monitor secret access with Datadog security signals.

**Integration:** AWS (EKS, Secrets Manager), Kubernetes, Angular, Terraform, FluxCD, Datadog.  
**Automation:** FluxCD syncs secrets; CSI automates Secrets Manager integration.

---

## Scenario 8: Multi-Tenant Kafka Processing with Namespace Isolation
**Original Task:** Namespaces  
**Theory:** Explore Namespaces’ role in multi-tenancy, resource quotas, RBAC scoping, and network isolation.  
**Use Case:** Isolate Kafka consumers for multiple tenants on EKS with AWS MSK.

**Hands-On:**
- Create `tenant-a` and `tenant-b` namespaces with `kubectl create namespace`.
- Deploy a Spring Boot Kafka consumer in each namespace, consuming from MSK topics.
- Test isolation by querying resources across namespaces.

**Mini-Project:**
- Terraform namespaces and MSK topics with tenant-specific IAM policies.
- Use FluxCD to deploy tenant-specific manifests.
- Monitor tenant metrics separately with Datadog tags.

**Integration:** AWS (EKS, MSK), Kubernetes, Spring Boot, Kafka, Terraform, FluxCD, Datadog.  
**Automation:** Terraform provisions resources; FluxCD ensures tenant-specific deployments.

---

## Scenario 9: Resource-Optimized Data Processing Pipeline
**Original Task:** Resource Requests and Limits  
**Theory:** Master resource requests/limits, Quality of Service (QoS) classes, eviction policies, and VerticalPodAutoscaler (VPA).  
**Use Case:** Optimize a Kafka Streams app running on EKS for resource efficiency.

**Hands-On:**
- Define a Deployment with resource requests (CPU: 100m, Memory: 256Mi) and limits (CPU: 500m, Memory: 1Gi).
- Stress-test with a Kafka producer and observe pod eviction.
- Adjust limits dynamically with `kubectl edit deployment`.

**Mini-Project:**
- Terraform an EKS cluster with Karpenter for node autoscaling.
- Deploy Kafka Streams with VPA and monitor with Datadog metrics.
- Automate resource tuning via FluxCD updates triggered by load.

**Integration:** AWS (EKS, MSK), Kubernetes, Kafka, Terraform, Karpenter, FluxCD, Datadog.  
**Automation:** Karpenter scales nodes; VPA and FluxCD adjust pod resources dynamically.

---

## Scenario 10: Troubleshooting a Production Kafka Consumer Failure
**Original Task:** Basic Troubleshooting  
**Theory:** Learn advanced debugging with kubectl (logs, events, describe, exec), Kubernetes events, and Pod lifecycle states.  
**Use Case:** Diagnose and fix a failing Spring Boot Kafka consumer on EKS integrated with AWS MSK.

**Hands-On:**
- Deploy a misconfigured consumer (e.g., wrong broker URL) and use `kubectl logs` to identify errors.
- Debug with `kubectl describe pod` and `kubectl exec` into the container.
- Fix the configuration and redeploy.

**Mini-Project:**
- Terraform the EKS cluster and MSK setup with automated monitoring.
- Use FluxCD to deploy the consumer and simulate a failure (e.g., network timeout).
- Resolve with Datadog traces and logs, automating alerts via Datadog monitors.

**Integration:** AWS (EKS, MSK), Kubernetes, Spring Boot, Kafka, Terraform, FluxCD, Datadog.  
**Automation:** FluxCD redeploys fixes; Datadog auto-alerts on failures.

---

# Key Features of These Scenarios

- **Advanced Concepts:** Each scenario incorporates multiple Kubernetes concepts (e.g., RBAC, HPA, VPA) and ecosystem tools.
- **AWS Integration:** Leverages AWS services (EKS, MSK, ALB, Secrets Manager) for real-world cloud-native scenarios.
- **Automation:** Terraform provisions infrastructure, FluxCD handles deployments, and tools like Karpenter and Istio automate scaling and traffic management.
- **Monitoring:** Datadog provides observability with metrics, logs, and traces, enhancing troubleshooting and optimization.
- **Ecosystem Tools:** Integrates Kafka, Spring Boot, Angular, and Istio to simulate a full-stack environment.

# How to Execute

1. **Setup:**  
   Use an AWS account with EKS enabled. Install Minikube locally for initial testing, then transition to EKS.

2. **Tools:**  
   Install Terraform, kubectl, eksctl, FluxCD CLI, Helm, and the Datadog agent.

3. **Automation Workflow:**  
   - Write Terraform scripts in a Git repo.  
   - Use FluxCD to sync Kubernetes manifests from the same repo.  
   - Configure Datadog for continuous monitoring.

---

## Scenario 11: Multi-Tenant External Access with Automated Ingress Management
**Original Task:** Ingress and External Access  
**Theory:** Dive into Ingress resources, controllers (NGINX, Traefik), path-based routing, TLS termination, and AWS ALB ingress controller specifics.  
**Use Case:** Provide external access to a multi-tenant Spring Boot application on EKS with automated ingress provisioning.

**Hands-On:**
- Install the AWS Load Balancer Controller using Terraform and FluxCD.
- Create an Ingress resource with path-based routing (`/tenant-a`, `/tenant-b`) for Spring Boot services.
- Configure annotations for AWS ALB (e.g., subnets, certificates).

**Mini-Project:**
- Automate Ingress deployment with FluxCD syncing from a Git repo.
- Integrate with AWS Route 53 for custom DNS (tenant-a.example.com).
- Monitor ingress traffic with Datadog APM and set up alerts for high latency.

**Integration:** AWS (EKS, ALB, Route 53), Kubernetes, Spring Boot, Terraform, FluxCD, Datadog.  
**Automation:** Terraform provisions the controller; FluxCD syncs Ingress manifests dynamically.

---

## Scenario 12: Secure Kafka Consumer Isolation with Network Policies
**Original Task:** Network Policies  
**Theory:** Master NetworkPolicies, CIDR-based rules, pod/namespace selectors, ingress/egress controls, and integration with service meshes like Istio.  
**Use Case:** Isolate Kafka consumer Pods on EKS to restrict access from unauthorized services while integrating with AWS MSK.

**Hands-On:**
- Deploy a Spring Boot Kafka consumer and a frontend app in separate namespaces on EKS.
- Create a NetworkPolicy to allow only frontend Pods to reach the consumer Pod on port 8080.
- Test isolation with `kubectl exec` and network tools (e.g., curl).

**Mini-Project:**
- Use Terraform to provision EKS and MSK with VPC peering for Kafka connectivity.
- Automate NetworkPolicy deployment with FluxCD and validate with Istio telemetry.
- Monitor network traffic with Datadog to ensure policy enforcement.

**Integration:** AWS (EKS, MSK), Kubernetes, Spring Boot, Kafka, Terraform, FluxCD, Istio, Datadog.  
**Automation:** FluxCD applies policies; Istio enhances traffic control; Datadog auto-verifies compliance.

---

## Scenario 13: Granular RBAC for Multi-Team Kubernetes Operations
**Original Task:** RBAC - Roles and RoleBindings  
**Theory:** Explore RBAC components (Roles, RoleBindings, ClusterRoles), role aggregation, verb granularity, and audit trails.  
**Use Case:** Implement team-specific access controls for managing Spring Boot microservices on EKS.

**Hands-On:**
- Define a Role allowing get, list, and update on Pods in the team-a namespace.
- Bind it to a user with a RoleBinding and test with `kubectl auth can-i`.
- Simulate restricted actions (e.g., delete) and verify denial.

**Mini-Project:**
- Use Terraform to create EKS namespaces and IAM roles mapped to Kubernetes RBAC via IRSA.
- Automate RBAC deployment with FluxCD syncing from Git.
- Log RBAC violations with Datadog security monitoring and impersonate users for testing.

**Integration:** AWS (EKS, IAM), Kubernetes, Spring Boot, Terraform, FluxCD, Datadog.  
**Automation:** Terraform maps IAM to RBAC; FluxCD syncs roles; Datadog auto-logs violations.

---

## Scenario 14: ServiceAccount-Driven Automation for Kafka Producers
**Original Task:** ServiceAccounts and RBAC  
**Theory:** Study ServiceAccounts, token generation, RBAC binding, IRSA (IAM Roles for Service Accounts), and pod identity management.  
**Use Case:** Automate a Kafka producer Pod on EKS with secure AWS MSK access via a ServiceAccount.

**Hands-On:**
- Create a ServiceAccount and bind it to a Role allowing create on Pods and ConfigMaps.
- Deploy a Spring Boot Kafka producer Pod with the ServiceAccount, publishing to MSK.
- Verify permissions with `kubectl describe pod` and test Kafka connectivity.

**Mini-Project:**
- Terraform an EKS cluster with IRSA linking the ServiceAccount to an AWS IAM role for MSK access.
- Use FluxCD to deploy the producer and automate ConfigMap updates for Kafka settings.
- Monitor Pod permissions and MSK traffic with Datadog traces.

**Integration:** AWS (EKS, MSK, IAM), Kubernetes, Spring Boot, Kafka, Terraform, FluxCD, Datadog.  
**Automation:** IRSA automates IAM access; FluxCD syncs manifests; Datadog tracks usage.

---

## Scenario 15: Enforcing Pod Security in a Multi-Tenant Environment
**Original Task:** Pod Security  
**Theory:** Master Pod Security Admission (PSA), PodSecurityPolicies (deprecated), security contexts, privilege escalation prevention, and seccomp profiles.  
**Use Case:** Enforce strict security policies for a multi-tenant EKS cluster hosting Angular and Spring Boot apps.

**Hands-On:**
- Configure PSA with restricted mode to block privileged Pods in a tenant-a namespace.
- Deploy a non-compliant Pod (e.g., `privileged: true`) and verify rejection.
- Set pod security context (e.g., `runAsNonRoot: true`, `allowPrivilegeEscalation: false`).

**Mini-Project:**
- Use Terraform to apply cluster-wide PSA labels and namespaces via EKS policies.
- Automate secure Pod deployment with FluxCD, integrating with AWS Secrets Manager for credentials.
- Audit violations with Datadog security signals and enforce compliance.

**Integration:** AWS (EKS, Secrets Manager), Kubernetes, Angular, Spring Boot, Terraform, FluxCD, Datadog.  
**Automation:** Terraform sets PSA; FluxCD deploys compliant Pods; Datadog auto-audits.

---

## Scenario 16: Auditing Kafka Consumer Access in a Secure Cluster
**Original Task:** Kubernetes Auditing  
**Theory:** Explore audit policies, log levels (Metadata, Request, RequestResponse), dynamic audit webhooks, and integration with observability tools.  
**Use Case:** Audit API calls for a Kafka consumer Pod on EKS to track unauthorized access attempts.

**Hands-On:**
- Enable auditing on EKS with a custom policy logging RequestResponse for Pod actions.
- Review audit logs in `/var/log/kubernetes/audit.log` or an S3 bucket sink.
- Simulate an unauthorized `kubectl delete pod` and verify logging.

**Mini-Project:**
- Terraform auditing setup with an S3 backend and IAM permissions.
- Deploy a Spring Boot Kafka consumer via FluxCD and trigger audit events.
- Stream audit logs to Datadog for real-time analysis and alerting.

**Integration:** AWS (EKS, S3, IAM), Kubernetes, Spring Boot, Kafka, Terraform, FluxCD, Datadog.  
**Automation:** Terraform configures auditing; FluxCD deploys apps; Datadog auto-processes logs.

---

## Scenario 17: Securing an Angular Frontend with Automated HTTPS
**Original Task:** Securing Ingress with HTTPS  
**Theory:** Study TLS certificates, cert-manager, Let’s Encrypt integration, HTTP-to-HTTPS redirects, and AWS ACM integration.  
**Use Case:** Secure an Angular frontend on EKS with HTTPS, integrated with AWS ALB and Route 53.

**Hands-On:**
- Install cert-manager with Helm and configure a ClusterIssuer for Let’s Encrypt.
- Create an Ingress for Angular with TLS annotations and apply a certificate.
- Test HTTPS access with `curl --insecure` and verify redirection.

**Mini-Project:**
- Terraform an EKS cluster with Route 53 DNS and ALB ingress controller.
- Automate Ingress and cert deployment with FluxCD syncing from Git.
- Monitor certificate health and HTTPS traffic with Datadog synthetics.

**Integration:** AWS (EKS, ALB, Route 53, ACM), Kubernetes, Angular, Terraform, FluxCD, cert-manager, Datadog.  
**Automation:** FluxCD syncs Ingress; cert-manager auto-renews certificates; Datadog verifies uptime.

---

## Scenario 18: Custom DNS for Multi-Tier Microservices
**Original Task:** Kubernetes DNS  
**Theory:** Master CoreDNS architecture, custom DNS configurations, stub domains, DNS resolution troubleshooting, and external DNS integration.  
**Use Case:** Configure custom DNS for a multi-tier app (Angular frontend, Spring Boot backend) on EKS with AWS Route 53.

**Hands-On:**
- Modify CoreDNS ConfigMap to add a stub domain (e.g., `internal.example.com`).
- Deploy Services for Angular and Spring Boot with custom DNS names.
- Test resolution with `kubectl exec` and `nslookup`.

**Mini-Project:**
- Terraform EKS with Route 53 integration for external DNS records.
- Use FluxCD to deploy services and ExternalDNS to sync DNS entries.
- Monitor DNS queries with Datadog logs and traces from CoreDNS.

**Integration:** AWS (EKS, Route 53), Kubernetes, Angular, Spring Boot, Terraform, FluxCD, ExternalDNS, Datadog.  
**Automation:** ExternalDNS syncs DNS; FluxCD deploys services; Datadog auto-logs DNS activity.

---

## Scenario 19: Load-Balanced Kafka Consumer with Autoscaling
**Original Task:** LoadBalancer Services  
**Theory:** Explore LoadBalancer Services, cloud provider integrations (AWS NLB/ALB), session affinity, health checks, and traffic distribution.  
**Use Case:** Deploy a load-balanced Kafka consumer on EKS with AWS NLB and auto-scaling based on traffic.

**Hands-On:**
- Create a LoadBalancer Service for a Spring Boot Kafka consumer Deployment.
- Configure AWS NLB annotations for external traffic (e.g., health checks).
- Simulate traffic with a Kubernetes Job producing Kafka messages.

**Mini-Project:**
- Terraform an EKS cluster with Karpenter for node autoscaling and NLB setup.
- Automate deployment with FluxCD and set up HPA based on Datadog custom metrics (e.g., consumer lag).
- Verify load balancing and scaling with Datadog dashboards.

**Integration:** AWS (EKS, NLB), Kubernetes, Spring Boot, Kafka, Terraform, Karpenter, FluxCD, Datadog.  
**Automation:** Karpenter scales nodes; HPA adjusts replicas; FluxCD syncs manifests.

---

## Scenario 20: Secure Multi-Tier Application with Istio and Kafka
**Original Task:** Multi-Tier Application Networking  
**Theory:** Review multi-tier architectures, service-to-service communication, mTLS with Istio, NetworkPolicies, and observability.  
**Use Case:** Deploy a secure multi-tier app (Angular frontend, Spring Boot backend, Kafka messaging) on EKS with Istio and AWS MSK.

**Hands-On:**
- Deploy Angular and Spring Boot with ClusterIP Services in separate namespaces.
- Configure Istio VirtualServices for routing and mTLS between services.
- Apply NetworkPolicies to restrict backend access to frontend only.

**Mini-Project:**
- Terraform an EKS cluster with Istio and MSK integration via VPC peering.
- Automate deployment with FluxCD, including Kafka producer/consumer in Spring Boot.
- Monitor end-to-end traffic with Datadog APM and Istio telemetry.

**Integration:** AWS (EKS, MSK), Kubernetes, Angular, Spring Boot, Kafka, Istio, Terraform, FluxCD, Datadog.  
**Automation:** FluxCD deploys all components; Istio secures traffic; Datadog auto-tracks performance.

---

# Key Features of These Scenarios

- **Advanced Concepts:** Incorporates RBAC scoping, Istio mTLS, PSA enforcement, and CoreDNS customization for production-grade complexity.
- **AWS Integration:** Leverages EKS, ALB/NLB, MSK, Route 53, Secrets Manager, and IAM for a cloud-native approach.
- **Automation:** Terraform provisions infrastructure, FluxCD syncs manifests, and tools like Karpenter and cert-manager automate scaling and security.
- **Ecosystem Tools:** Integrates Kafka, Spring Boot, Angular, Istio, and Datadog to simulate a full-stack environment.
- **Monitoring:** Datadog provides deep observability with metrics, logs, traces, and security signals.

---

# Execution Guide

## Setup
- Use an AWS account with EKS, Route 53, and MSK enabled.
- Start with Minikube for local testing, then scale to EKS.

## Tools
- Install Terraform, kubectl, eksctl, FluxCD CLI, Helm, Istioctl, and the Datadog agent.

## Workflow
1. **Infrastructure as Code:**  
   Write Terraform scripts in a Git repository to provision AWS infrastructure.

2. **GitOps Deployment:**  
   Configure FluxCD to sync Kubernetes manifests and Helm charts from the same repo.

3. **Monitoring and Observability:**  
   Set up Datadog for continuous monitoring, logging, and alerting.

--- 

### Scenario 21: Distributed Log Collection and Batch Processing for Kafka Streams
**Original Task:** DaemonSets and Jobs  
**Theory:** Dive into DaemonSets for node-level workloads, Jobs for batch processing, parallelism, completions, and backoff policies.  
**Use Case:** Deploy a distributed log collection system on EKS with batch processing of Kafka messages.

#### Hands-On:
- Deploy a DaemonSet running Fluentd to collect logs from all EKS nodes and forward them to AWS CloudWatch.
- Create a Job to process a backlog of Kafka messages using a Spring Boot consumer.
- Verify Job completion with `kubectl get jobs` and check logs in CloudWatch.

#### Mini-Project:
- Use Terraform to provision EKS and AWS MSK with VPC peering.
- Automate DaemonSet and Job deployment with FluxCD syncing from a Git repo.
- Monitor log ingestion and Job execution with Datadog dashboards.

**Integration:** AWS (EKS, MSK, CloudWatch), Kubernetes, Spring Boot, Kafka, Fluentd, Terraform, FluxCD, Datadog.  
**Automation:** FluxCD syncs manifests; Fluentd auto-collects logs; Datadog tracks Job status.

---

### Scenario 22: Auto-Scaling a Real-Time Analytics Service with HPA
**Original Task:** Horizontal Pod Autoscaling (HPA)  
**Theory:** Master HPA mechanics, metrics server integration, scaling thresholds, stabilization windows, and multi-metric policies.  
**Use Case:** Scale a Spring Boot analytics service on EKS processing Kafka streams based on CPU load.

#### Hands-On:
- Deploy a Spring Boot app with Kafka consumer logic and configure HPA to scale on CPU (target: 70%).
- Install the Kubernetes Metrics Server with Helm.
- Simulate CPU load with a Kafka producer Job and observe scaling with `kubectl get hpa`.

#### Mini-Project:
- Terraform an EKS cluster with Karpenter for node scaling and integrate with AWS ALB.
- Automate HPA deployment via FluxCD and monitor scaling events with Datadog.
- Validate zero-downtime scaling during load spikes.

**Integration:** AWS (EKS, MSK, ALB), Kubernetes, Spring Boot, Kafka, Terraform, Karpenter, FluxCD, Datadog.  
**Automation:** Karpenter adds nodes; FluxCD applies HPA; Datadog alerts on scaling thresholds.

---

### Scenario 23: Optimizing Resource Allocation for a Machine Learning Workload
**Original Task:** Vertical Pod Autoscaling (VPA)  
**Theory:** Explore VPA mechanics, recommendation modes, update modes (auto, recreate), resource estimation, and eviction handling.  
**Use Case:** Optimize a TensorFlow training Pod on EKS for dynamic resource allocation.

#### Hands-On:
- Install VPA with Helm and apply it to a TensorFlow Pod with initial requests (CPU: 200m, Memory: 512Mi).
- Run a training workload and monitor VPA recommendations with `kubectl describe vpa`.
- Switch to auto mode and observe Pod recreation with updated resources.

#### Mini-Project:
- Terraform an EKS cluster with GPU-enabled nodes (e.g., g4dn instances).
- Automate VPA deployment with FluxCD and sync training manifests from Git.
- Track resource usage and VPA adjustments with Datadog custom metrics.

**Integration:** AWS (EKS), Kubernetes, TensorFlow, Terraform, FluxCD, Datadog.  
**Automation:** FluxCD syncs VPA; Datadog auto-reports resource optimization trends.

---

### Scenario 24: Custom Metrics-Driven Scaling for an API Gateway
**Original Task:** Custom Metrics for HPA  
**Theory:** Study custom metrics, Prometheus adapter, external metrics APIs, metric pipelines, and scaling on application-specific signals.  
**Use Case:** Scale a Spring Boot API gateway on EKS based on HTTP requests per second, integrated with AWS ALB.

#### Hands-On:
- Install Prometheus and the Prometheus Adapter with Helm on EKS.
- Configure HPA to scale on a custom metric (`http_requests_total`) with a target of 100 req/s per pod.
- Simulate load with a Kubernetes Job hitting the API and verify scaling with `kubectl get hpa`.

#### Mini-Project:
- Terraform an EKS cluster with ALB ingress and Prometheus stack deployment via FluxCD.
- Export API metrics with Spring Boot Actuator and Micrometer to Prometheus.
- Monitor scaling and request rates with Datadog dashboards synced from Prometheus.

**Integration:** AWS (EKS, ALB), Kubernetes, Spring Boot, Prometheus, Terraform, FluxCD, Datadog.  
**Automation:** FluxCD deploys metrics stack; HPA auto-scales; Datadog visualizes custom metrics.

---

### Scenario 25: Multi-Tenant Resource Governance for Kafka Consumers
**Original Task:** Resource Quotas and LimitRanges  
**Theory:** Master ResourceQuotas (CPU, memory, object counts), LimitRanges (default/min/max limits), QoS classes, and namespace scoping.  
**Use Case:** Enforce resource governance for Kafka consumers across tenant namespaces on EKS with AWS MSK.

#### Hands-On:
- Apply a ResourceQuota (CPU: 2, Memory: 4Gi) and LimitRange (max CPU: 1, max Memory: 2Gi) to a `tenant-a` namespace.
- Deploy a Spring Boot Kafka consumer and test quota enforcement with `kubectl describe quota`.
- Overload with a resource-heavy Pod and observe rejection or throttling.

#### Mini-Project:
- Terraform EKS with tenant namespaces and MSK integration.
- Automate quota/limit deployment with FluxCD and sync consumer manifests.
- Monitor resource usage and quota violations with Datadog tags per tenant.

**Integration:** AWS (EKS, MSK), Kubernetes, Spring Boot, Kafka, Terraform, FluxCD, Datadog.  
**Automation:** Terraform sets quotas; FluxCD applies manifests; Datadog alerts on overages.

---

## Scenario 26: Ambassador Proxy for External Kafka Integration
**Original Task:** Multi-Container Patterns  
**Theory:** Explore ambassador, adapter, and sidecar patterns, container networking, shared volumes, and lifecycle coordination.  
**Use Case:** Use an ambassador proxy in a Pod on EKS to route traffic to an external AWS MSK cluster securely.  

### Hands-On:
- Deploy a Pod with a Spring Boot Kafka consumer and an Envoy proxy as an ambassador container.
- Configure Envoy to route Kafka traffic to MSK with TLS encryption.
- Test connectivity with `kubectl exec` and verify message consumption.

### Mini-Project:
- Terraform EKS and MSK with VPC peering and IAM roles for secure access.
- Automate Pod deployment with FluxCD and Istio for enhanced traffic management.
- Monitor proxy performance and Kafka latency with Datadog APM.

**Integration:** AWS (EKS, MSK), Kubernetes, Spring Boot, Kafka, Envoy, Istio, Terraform, FluxCD, Datadog.  
**Automation:** FluxCD syncs Pod; Istio optimizes routing; Datadog auto-tracks metrics.  

---

## Scenario 27: Zero-Downtime Updates for an Angular Frontend
**Original Task:** Rolling Updates  
**Theory:** Study rolling update strategies (`maxUnavailable`, `maxSurge`), readiness probes, rollback mechanisms, and update orchestration.  
**Use Case:** Perform a zero-downtime rolling update for an Angular app on EKS with AWS ALB.  

### Hands-On:
- Deploy an Angular app with a Deployment using strategy: RollingUpdate (`maxUnavailable: 0`, `maxSurge: 1`).
- Update the image tag (e.g., `nginx:1.19` to `nginx:1.20`) and monitor with `kubectl rollout status`.
- Simulate a failed update by breaking readiness probes and roll back with `kubectl rollout undo`.

### Mini-Project:
- Terraform an EKS cluster with ALB ingress and deploy via FluxCD.
- Automate image updates with FluxCD ImagePolicy and validate with Datadog synthetics.
- Ensure zero downtime with Istio traffic mirroring during updates.

**Integration:** AWS (EKS, ALB), Kubernetes, Angular, Terraform, FluxCD, Istio, Datadog.  
**Automation:** FluxCD triggers updates; Istio ensures continuity; Datadog verifies uptime.  

---

## Scenario 28: Canary Release of a Spring Boot Microservice with Kafka
**Original Task:** Canary Deployments  
**Theory:** Explore canary release strategies, traffic splitting with labels/selectors, Istio/Kubernetes-native approaches, and validation metrics.  
**Use Case:** Roll out a new Spring Boot microservice version on EKS with Kafka integration using a canary deployment.  

### Hands-On:
- Deploy two Spring Boot versions (`v1`, `v2`) with distinct labels and a shared Service.
- Use Istio VirtualService to split traffic (90% `v1`, 10% `v2`) and test with Kafka message consumption.
- Gradually shift traffic to `v2` with `kubectl apply` and Istio weights.

### Mini-Project:
- Terraform EKS with MSK and Istio installed via FluxCD.
- Automate canary rollout with FluxCD and monitor success with Datadog traces.
- Roll back if `v2` fails Kafka processing benchmarks.

**Integration:** AWS (EKS, MSK), Kubernetes, Spring Boot, Kafka, Istio, Terraform, FluxCD, Datadog.  
**Automation:** FluxCD deploys versions; Istio splits traffic; Datadog validates performance.  

---

## Scenario 29: Blue-Green Deployment for a Critical Data Pipeline
**Original Task:** Blue-Green Deployments  
**Theory:** Study blue-green deployment concepts, service switching, zero-downtime strategies, and rollback orchestration.  
**Use Case:** Deploy a Kafka Streams data pipeline on EKS with blue-green switching integrated with AWS MSK.  

### Hands-On:
- Deploy two Kafka Streams Deployments (`blue` and `green`) with separate labels.
- Use a Service with a selector to point to `blue`, then switch to `green` by updating the selector.
- Verify seamless switch with `kubectl get endpoints` and Kafka throughput.

### Mini-Project:
- Terraform EKS and MSK with an ALB for external validation traffic.
- Automate deployment and switching with FluxCD Kustomization overlays.
- Monitor pipeline health and switch success with Datadog metrics and logs.

**Integration:** AWS (EKS, MSK, ALB), Kubernetes, Kafka, Terraform, FluxCD, Datadog.  
**Automation:** FluxCD manages deployments; Datadog triggers alerts for rollback if needed.  

---

## Scenario 30: Automating Observability with a Prometheus Operator
**Original Task:** Kubernetes Operators  
**Theory:** Master Operators, Custom Resource Definitions (CRDs), controller patterns, reconciliation loops, and Operator SDK.  
**Use Case:** Deploy and manage a Prometheus instance on EKS with an Operator for monitoring a Spring Boot app.  

### Hands-On:
- Install the Prometheus Operator with Helm and create a Prometheus CRD instance.
- Configure ServiceMonitors to scrape Spring Boot metrics via Actuator.
- Observe Operator behavior with `kubectl describe prometheus` and verify metrics in Prometheus UI.

### Mini-Project:
- Terraform an EKS cluster and deploy the Operator via FluxCD.
- Automate Spring Boot monitoring with FluxCD syncing ServiceMonitors.
- Integrate with Datadog for unified dashboards and alerts synced from Prometheus.

**Integration:** AWS (EKS), Kubernetes, Spring Boot, Prometheus, Terraform, FluxCD, Datadog.  
**Automation:** FluxCD deploys Operator and CRDs; Datadog aggregates metrics automatically.  

---

## Key Features of These Scenarios
- **Advanced Concepts:** Incorporates DaemonSets for distributed tasks, HPA/VPA with custom metrics, multi-container orchestration, and Operator-driven automation.
- **AWS Integration:** Leverages EKS, MSK, ALB, CloudWatch, and IAM for cloud-native scalability and reliability.
- **Automation:** Terraform provisions infrastructure, FluxCD syncs workloads, and tools like Karpenter and Istio automate scaling and traffic management.
- **Ecosystem Tools:** Integrates Kafka, Spring Boot, TensorFlow, Istio, and Datadog for a full-stack production environment.
- **Monitoring:** Datadog provides comprehensive observability with metrics, logs, traces, and synthetic tests.

---

## Execution Guide
### Setup:
- Use an AWS account with EKS and MSK enabled. Start with Minikube locally for prototyping, then scale to EKS.

### Tools:
- Install Terraform, `kubectl`, `eksctl`, FluxCD CLI, Helm, Istioctl, and Datadog agent.

### Workflow:
1. Script infrastructure with Terraform in a Git repo.
2. Use FluxCD to sync Kubernetes manifests and Helm charts from the repo.
3. Configure Datadog for real-time monitoring and alerting.
4. Leverage Istio for intelligent traffic management and security.

--- 
## Scenario 31: Automated Storage Provisioning for Multi-Tenant Apps
**Original Task:** StorageClasses  
**Theory:** Dive into StorageClasses, provisioners (e.g., EBS, EFS), parameters (e.g., IOPS, encryption), dynamic provisioning, and reclaim policies.  
**Use Case:** Automate storage provisioning for multi-tenant Spring Boot apps on EKS with AWS EBS.

### Hands-On:
- Create a StorageClass with the aws-ebs provisioner, encrypted volumes, and gp3 type (3000 IOPS).
- Deploy a PVC referencing the StorageClass and mount it to a Spring Boot Pod.
- Verify volume creation with `kubectl get pvc` and via the AWS console.

### Mini-Project:
- Use Terraform to provision EKS and define StorageClasses for `tenant-a` and `tenant-b`.
- Automate PVC deployment with FluxCD syncing from a Git repo.
- Monitor storage usage with Datadog custom metrics tagged by tenant.

**Integration:** AWS (EKS, EBS), Kubernetes, Spring Boot, Terraform, FluxCD, Datadog.  
**Automation:** Terraform provisions StorageClasses; FluxCD syncs PVCs; Datadog auto-tracks capacity.

---

## Scenario 32: Dynamic Volume Provisioning for a Kafka Producer
**Original Task:** Dynamic Volume Provisioning  
**Theory:** Explore dynamic provisioning workflows, cloud provider integration (e.g., AWS EBS), volume binding modes, and topology awareness.  
**Use Case:** Provision dynamic storage for a Kafka producer on EKS to persist message logs with AWS EBS.

### Hands-On:
- Deploy a StorageClass with aws-ebs and a PVC requesting 10Gi.
- Mount the PVC to a Spring Boot Kafka producer Pod and write logs to it.
- Resize the PVC to 20Gi with `kubectl edit pvc` and verify expansion in AWS.

### Mini-Project:
- Terraform an EKS cluster with the EBS CSI driver and MSK integration.
- Automate PVC and Pod deployment with FluxCD, syncing from Git.
- Monitor volume resizing and Kafka throughput with Datadog dashboards.

**Integration:** AWS (EKS, EBS, MSK), Kubernetes, Spring Boot, Kafka, Terraform, FluxCD, Datadog.  
**Automation:** FluxCD applies PVCs; EBS CSI auto-provisions; Datadog alerts on resizing events.

---

## Scenario 33: Stateful Kafka Consumer with Persistent Storage
**Original Task:** StatefulSets  
**Theory:** Master StatefulSet mechanics, stable network identities, ordinal indexing, pod management policies, and volumeClaimTemplates.  
**Use Case:** Deploy a stateful Kafka consumer on EKS with persistent storage for offset tracking using AWS EBS.

### Hands-On:
- Create a StatefulSet for a Spring Boot Kafka consumer with a `volumeClaimTemplates` section (10Gi EBS).
- Scale it to 3 replicas with `kubectl scale statefulset` and check ordinal Pod names (e.g., `consumer-0`).
- Verify persistent storage with `kubectl describe pvc`.

### Mini-Project:
- Terraform EKS and MSK with VPC peering for Kafka connectivity.
- Automate StatefulSet deployment with FluxCD and monitor with Datadog.
- Simulate consumer failover and validate offset persistence across scaling.

**Integration:** AWS (EKS, EBS, MSK), Kubernetes, Spring Boot, Kafka, Terraform, FluxCD, Datadog.  
**Automation:** FluxCD syncs StatefulSet; Datadog tracks pod stability and storage usage.

---

## Scenario 34: High-Availability PostgreSQL with Kubernetes
**Original Task:** Deploying a Database  
**Theory:** Study database best practices on Kubernetes—StatefulSets, replication, connection pooling, backups, and pod anti-affinity.  
**Use Case:** Deploy a highly available PostgreSQL cluster on EKS with AWS RDS as a reference point.

### Hands-On:
- Deploy PostgreSQL with a StatefulSet, using `volumeClaimTemplates` for data persistence (EBS).
- Configure a Spring Boot app Pod to connect to PostgreSQL via JDBC.
- Test connectivity with `kubectl exec` and `psql`.

### Mini-Project:
- Terraform EKS with the EBS CSI driver and deploy PostgreSQL via FluxCD.
- Set up pod anti-affinity to spread replicas across nodes.
- Monitor database performance and connection latency with Datadog APM.

**Integration:** AWS (EKS, EBS), Kubernetes, PostgreSQL, Spring Boot, Terraform, FluxCD, Datadog.  
**Automation:** FluxCD deploys the database; Datadog auto-monitors health and connections.

---

## Scenario 35: Service Discovery for a Distributed PostgreSQL Cluster
**Original Task:** Headless Services  
**Theory:** Explore headless Services, DNS A records, StatefulSet integration, client-side load balancing, and service discovery patterns.  
**Use Case:** Enable direct Pod access for a PostgreSQL cluster on EKS with AWS EBS storage.

### Hands-On:
- Create a headless Service (`clusterIP: None`) for a PostgreSQL StatefulSet.
- Use `kubectl get endpoints` to verify individual Pod IPs.
- Resolve Pod DNS entries (e.g., `postgres-0.postgres.default.svc.cluster.local`) with `nslookup` from another Pod.

### Mini-Project:
- Terraform EKS with ExternalDNS for Route 53 integration.
- Automate StatefulSet and headless Service deployment with FluxCD.
- Monitor DNS resolution and Pod availability with Datadog logs from CoreDNS.

**Integration:** AWS (EKS, EBS, Route 53), Kubernetes, PostgreSQL, Terraform, FluxCD, ExternalDNS, Datadog.  
**Automation:** FluxCD syncs manifests; ExternalDNS updates Route 53; Datadog tracks DNS queries.

---

## Scenario 36: Backup and Recovery for a Stateful Kafka Consumer
**Original Task:** Volume Snapshots  
**Theory:** Study Volume Snapshots, CSI snapshotter, snapshot controllers, restore workflows, and backup strategies.  
**Use Case:** Implement backup and recovery for a Kafka consumer’s persistent storage on EKS with AWS EBS.

### Hands-On:
- Install the CSI Snapshot Controller and create a VolumeSnapshotClass for EBS.
- Take a snapshot of a Kafka consumer’s PVC with `kubectl apply -f snapshot.yaml`.
- Restore it to a new PVC and verify data integrity with `kubectl describe pvc`.

### Mini-Project:
- Terraform EKS with the EBS CSI driver and MSK integration.
- Automate snapshot scheduling with a Kubernetes CronJob synced by FluxCD.
- Monitor snapshot success and storage usage with Datadog dashboards.

**Integration:** AWS (EKS, EBS, MSK), Kubernetes, Kafka, Terraform, FluxCD, Datadog.  
**Automation:** A CronJob schedules snapshots; FluxCD deploys manifests; Datadog alerts on failures.

---

## Scenario 37: Custom Storage Provisioning with CSI Driver
**Original Task:** CSI Drivers  
**Theory:** Master the Container Storage Interface (CSI), driver architecture, volume lifecycle, custom provisioners, and CSI adoption in Kubernetes.  
**Use Case:** Provision custom EBS volumes for a Spring Boot app on EKS using the AWS EBS CSI driver.

### Hands-On:
- Install the AWS EBS CSI driver with Helm on EKS.
- Create a StorageClass with custom parameters (e.g., `fsType: ext4`, `encrypted: true`).
- Deploy a PVC and mount it to a Spring Boot Pod, verifying with `kubectl get pv`.

### Mini-Project:
- Terraform EKS with the CSI driver and IAM roles for EBS access.
- Automate StorageClass and PVC deployment with FluxCD.
- Monitor volume provisioning and IOPS with Datadog metrics.

**Integration:** AWS (EKS, EBS), Kubernetes, Spring Boot, Terraform, FluxCD, Datadog.  
**Automation:** FluxCD syncs CSI resources; Datadog auto-tracks volume performance.

---

## Scenario 38: Dynamic Volume Resizing for a Growing Dataset
**Original Task:** Resizing PersistentVolumes  
**Theory:** Explore PVC resizing, StorageClass `allowVolumeExpansion`, filesystem expansion, resizing limits, and cloud provider support.  
**Use Case:** Dynamically resize an EBS volume for a PostgreSQL database on EKS as data grows.

### Hands-On:
- Create a StorageClass with `allowVolumeExpansion: true` and a 10Gi PVC for PostgreSQL.
- Resize the PVC to 20Gi with `kubectl edit pvc` and verify expansion with `kubectl describe pv`.
- Test filesystem growth by writing data and checking with `kubectl exec`.

### Mini-Project:
- Terraform EKS with the EBS CSI driver and deploy PostgreSQL via FluxCD.
- Automate resizing triggers with a custom script synced by FluxCD, based on Datadog storage metrics.
- Validate resizing limits with varying StorageClass configurations.

**Integration:** AWS (EKS, EBS), Kubernetes, PostgreSQL, Terraform, FluxCD, Datadog.  
**Automation:** FluxCD applies PVC updates; Datadog triggers resizing based on usage thresholds.

---

## Scenario 39: Cross-Cluster Data Migration with Velero
**Original Task:** Data Migration  
**Theory:** Study data migration strategies, Velero backup/restore, Restic integration, cross-cluster workflows, and disaster recovery planning.  
**Use Case:** Migrate a Spring Boot app’s persistent data from one EKS cluster to another with AWS S3 backups.

### Hands-On:
- Install Velero with AWS S3 backend and configure Restic for filesystem backups.
- Back up a Spring Boot PVC with `velero backup create` and restore it to a new cluster with `velero restore`.
- Verify data consistency with `kubectl describe pvc` on the target cluster.

### Mini-Project:
- Terraform two EKS clusters and an S3 bucket with lifecycle policies.
- Automate backup schedules with Velero and FluxCD-managed CronJobs.
- Monitor migration success and backup integrity with Datadog logs.

**Integration:** AWS (EKS, S3), Kubernetes, Spring Boot, Terraform, FluxCD, Velero, Datadog.  
**Automation:** Velero schedules backups; FluxCD syncs manifests; Datadog validates restores.

---

## Scenario 40: Distributed Cassandra Cluster for Real-Time Analytics
**Original Task:** Distributed Databases  
**Theory:** Explore distributed database concepts—replication, consistency models (e.g., eventual consistency), sharding, and Kubernetes integration.  
**Use Case:** Deploy a Cassandra cluster on EKS for real-time analytics of Kafka streams with AWS EFS for shared storage.

### Hands-On:
- Deploy a Cassandra StatefulSet with 3 replicas and an EFS-backed PVC for data persistence.
- Insert sample data with `cqlsh` from a `kubectl exec` session and verify replication across nodes.
- Scale to 5 replicas and observe cluster rebalancing with `nodetool status`.

### Mini-Project:
- Terraform EKS with the EFS CSI driver and MSK for Kafka integration.
- Automate Cassandra deployment with FluxCD and monitor with Datadog metrics (e.g., read/write latency).
- Integrate with a Spring Boot app consuming Kafka streams and writing to Cassandra.

**Integration:** AWS (EKS, EFS, MSK), Kubernetes, Cassandra, Spring Boot, Kafka, Terraform, FluxCD, Datadog.  
**Automation:** FluxCD deploys the cluster; Datadog auto-tracks replication and performance.

## Key Features of These Scenarios
- **Advanced Concepts:** Incorporates StorageClasses with custom parameters, StatefulSets for stable identities, CSI drivers, Volume Snapshots, and distributed database replication.
- **AWS Integration:** Leverages EKS, EBS, EFS, MSK, S3, and Route 53 for cloud-native storage and stateful workloads.
- **Automation:** Terraform provisions infrastructure, FluxCD syncs manifests, and tools like Velero automate backups and migrations.
- **Ecosystem Tools:** Integrates Kafka, Spring Boot, PostgreSQL, Cassandra, and Datadog for a production-grade environment.
- **Monitoring:** Datadog provides deep observability with metrics, logs, and traces for storage and stateful apps.

---

## Execution Guide
### Setup:
- Use an AWS account with EKS, EBS, EFS, and MSK enabled. Start with Minikube locally for testing, then scale to EKS.

### Tools:
- Install Terraform, `kubectl`, `eksctl`, FluxCD CLI, Helm, Velero, and the Datadog agent.

### Workflow:
1. Script infrastructure with Terraform in a Git repo.
2. Use FluxCD to sync Kubernetes manifests and Helm charts from the repo.
3. Configure Datadog for real-time monitoring and alerting.
