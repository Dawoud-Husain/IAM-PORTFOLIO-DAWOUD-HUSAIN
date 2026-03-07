# 3.3 — Provision and Manage Containers in the Azure Portal

**Domain Weight: 20–25% (HIGH PRIORITY)**

---

## Core Concepts

### Create and Manage an Azure Container Registry

Azure Container Registry (ACR) is a managed, private Docker registry service for storing and managing container images and related artifacts (like Helm charts). ACR integrates with container deployment services like ACI, AKS, and Container Apps. You choose a SKU tier (Basic, Standard, or Premium) based on storage needs, throughput, and feature requirements.

**ACR SKU Comparison:**

| Feature | Basic | Standard | Premium |
| -------------------------------- | ----- | -------- | ------- |
| Included storage | 10 GiB | 100 GiB | 500 GiB |
| Max storage limit | 40 TiB | 40 TiB | 100 TiB |
| Webhooks | 2 | 10 | 500 |
| Geo-replication | NO | NO | YES |
| Private link / private endpoints | NO | NO | YES |
| Content trust | NO | NO | YES |
| Customer-managed keys | NO | NO | YES |
| Availability zones | YES | YES | YES |
| Anonymous pull access | NO | YES | YES |
| Retention policy (untagged) | NO | NO | YES |

**ACR Key Facts:**

- Registry name: **unique within Azure**, 5–50 alphanumeric characters (no dashes)
- Login server format: `<registryname>.azurecr.io`
- Authentication: Microsoft Entra ID, admin user (for testing only), service principal
- Admin user: disabled by default; enable only for testing/dev scenarios
- **SKU can be changed** freely (no downtime); must remove Premium features before downgrading
- All SKUs provide same programmatic capabilities and data plane APIs
- ACR Tasks: build images in Azure without local Docker (e.g., `az acr build`)

**Image Operations:**

- `docker push` / `az acr import` — push images to registry
- `docker pull` — pull images from registry
- `az acr login` — authenticate to registry (uses Azure CLI credentials)
- Images stored as managed Azure storage — no storage account needed

---

### Provision a Container by Using Azure Container Instances

Azure Container Instances (ACI) is the fastest and simplest way to run isolated containers in Azure without managing VMs or adopting a full orchestrator. ACI supports both Linux and Windows containers, provides per-second billing, and can start containers in seconds. The top-level resource is the **container group** — a collection of containers scheduled on the same host.

**Container Group:**

- Similar to a Kubernetes **pod**
- Shares lifecycle, networking (IP address), storage volumes
- Multi-container groups: **Linux only** (Windows supports single container only)
- Containers in the same group communicate via **localhost**
- Single public IP + FQDN per container group

**Creating ACI (Portal):**

1. Create a resource → Containers → Container Instances
2. Configure: subscription, RG, container name, region, image source, OS type
3. Image source: ACR, Docker Hub, or other registry
4. Set CPU/memory, networking (public IP, ports, DNS label)
5. Set restart policy, environment variables, volume mounts

**Restart Policies:**

| Policy | Behavior | Use Case |
| -------------- | ---------------------------------------- | ----------------------------------- |
| **Always** | Always restart (DEFAULT) | Long-running services (web servers) |
| **OnFailure** | Restart only on non-zero exit code | Task/batch jobs with error retry |
| **Never** | Run once, never restart | One-time tasks, scripts |

**Container Group States:**

| State | Description |
| ------------- | ------------------------------------------- |
| Running | Container group is actively executing |
| Stopped | Manually stopped, won't restart |
| Pending | Waiting to initialize |
| Succeeded | Ran to completion (Never/OnFailure policies) |
| Failed | Failed to run (Never policy only) |

**Networking Options:**

- **Public:** public IP + optional DNS label (`customlabel.region.azurecontainer.io`)
- **Private:** deploy into a VNet subnet
- **None:** no external access
- Ports per IP: max **5**

**Volume Mounts (supported):**

- Azure File share
- Secret volume
- Empty directory
- Git repo (cloned)

---

### Provision a Container by Using Azure Container Apps

Azure Container Apps is a serverless platform for running containerized microservices and jobs. It abstracts away infrastructure, provides built-in autoscaling via KEDA, service discovery, traffic splitting, and can scale to zero. Container Apps runs within a **Container Apps environment**, which is the secure boundary for a group of container apps.

**Key Container Apps Features:**

- Serverless — fully managed, no cluster to manage
- **Scale to zero** (no charges when scaled to 0)
- Built-in HTTPS/TCP ingress
- Traffic splitting across revisions (blue/green, A/B testing)
- Built-in service discovery (DNS-based)
- Dapr integration for microservices
- Supports running scheduled, on-demand, and event-driven **jobs**
- Powered by: **Kubernetes**, **KEDA**, **Envoy**
- Does NOT expose Kubernetes API (use AKS if you need K8s API access)
- Runs containers from any registry (Docker Hub, ACR, etc.)

**Container Apps Concepts:**

| Concept | Description |
| ------------------- | --------------------------------------------------- |
| **Environment** | Secure boundary; shared VNet, logging, Dapr config |
| **Container App** | Individual application within an environment |
| **Revision** | Immutable snapshot of a container app version |
| **Replica** | Instance of a revision (horizontal scaling) |

**Creating a Container App (Portal):**

1. Create a resource → Container Apps
2. Configure: subscription, RG, name, region
3. Create or select a Container Apps **environment**
4. Set container image, CPU/memory, ingress settings
5. Configure scaling rules (optional)

---

### Manage Sizing and Scaling for Containers, Including Azure Container Instances and Azure Container Apps

ACI and Container Apps handle sizing and scaling differently. ACI requires manual resource allocation and does not natively auto-scale — you must create separate container groups for more capacity. Container Apps provides built-in autoscaling via KEDA, scaling replicas based on HTTP traffic, events, CPU, or memory.

**ACI Sizing:**

- CPU and memory specified **per container group**
- Minimum: **1 CPU + 1 GB memory** per container group
- Maximum (standard): **31 CPU + 240 GB memory** per container group
- Storage per container group: **50 GB**
- GPU support: V100 (Linux only, 1/2/4 GPUs)
- Resources shared across containers in a group
- **No built-in auto-scaling** — create more container groups manually or via API
- To change CPU, memory, restart policy, or OS type → must **delete and recreate** the container group

**Container Apps Scaling:**

- **Horizontal scaling** (replicas) only — no vertical scaling
- Default: min 0, max 10 replicas
- Configurable: min 0, max **1,000** replicas
- Scale to zero: YES (no usage charges at 0)
- Apps that scale on CPU/memory **cannot scale to zero**

**Scale Rule Types:**

| Type | Trigger | Example |
| ---------- | ---------------------------------- | -------------------------------- |
| **HTTP** | Concurrent HTTP requests | 100 requests per second |
| **TCP** | Concurrent TCP connections | Based on connection count |
| **Custom** | Event-driven (KEDA scalers) | Azure Service Bus queue length |
| | CPU or memory load | CPU > 70% |

**Default Scale Rule (if none defined):**

| Trigger | Min Replicas | Max Replicas |
| ------- | ------------ | ------------ |
| HTTP | 0 | 10 |

**Scaling Behavior:**

- Scale-down stabilization window: **300 seconds**
- Cool-down period before scaling to 0: **300 seconds**
- Adding/editing scale rules creates a **new revision**

**Mnemonic — Container Scaling: "ACI = Manual, ACA = Auto"**

- **ACI:** no auto-scale, create more container groups manually
- **ACA:** KEDA-powered autoscale, min/max replicas, scale to zero

---

## What The Exam Tests

- ACR SKU differences: only **Premium** supports geo-replication, private link, content trust
- ACR registry name must be unique and 5–50 alphanumeric characters
- ACI multi-container groups are **Linux only**
- ACI restart policies: Always (default), OnFailure, Never
- ACI changes to CPU/memory/restart policy/OS require **delete and recreate**
- Container Apps can **scale to zero** (except CPU/memory-based triggers)
- Container Apps scaling is powered by **KEDA**
- Container Apps does NOT expose Kubernetes API
- Container Apps **revisions** are immutable snapshots
- ACI has no built-in auto-scaling; Container Apps does
- Difference between ACI (lower-level building block) vs Container Apps (higher-level serverless)
- Container group resources: min 1 CPU + 1 GB, max 31 CPU + 240 GB

---

## Important Limits / Numbers

| Item | Value |
| --------------------------------------------- | -------------------------------------------- |
| ACR name length | **5–50** alphanumeric characters |
| ACR Basic included storage | **10 GiB** |
| ACR Standard included storage | **100 GiB** |
| ACR Premium included storage | **500 GiB** |
| ACR Premium max storage | **100 TiB** |
| ACI min resources per container group | **1 CPU + 1 GB memory** |
| ACI max resources (standard) | **31 CPU + 240 GB memory** |
| ACI storage per container group | **50 GB** |
| ACI max containers per group | **60** |
| ACI max volumes per group | **20** |
| ACI ports per IP | **5** |
| ACI container groups per region/subscription | **100** (default) |
| Container Apps default max replicas | **10** |
| Container Apps configurable max replicas | **1,000** |
| Container Apps scale-down stabilization | **300 seconds** |
| Container Apps cool-down to zero | **300 seconds** |

---

## CLI + PowerShell Commands

### Azure CLI

```bash
# --- Azure Container Registry ---

# Create a registry
az acr create \
  --resource-group myRG \
  --name myregistry \
  --sku Standard

# Log in to registry
az acr login --name myregistry

# Build image in ACR (no local Docker needed)
az acr build \
  --registry myregistry \
  --image myapp:v1 .

# List images
az acr repository list --name myregistry --output table

# Show usage
az acr show-usage --name myregistry --output table

# Change SKU
az acr update --name myregistry --sku Premium

# --- Azure Container Instances ---

# Create a container instance
az container create \
  --resource-group myRG \
  --name mycontainer \
  --image mcr.microsoft.com/azuredocs/aci-helloworld \
  --cpu 1 --memory 1.5 \
  --ip-address Public --ports 80 \
  --restart-policy Always

# Show container details
az container show \
  --resource-group myRG \
  --name mycontainer \
  --output table

# View logs
az container logs \
  --resource-group myRG \
  --name mycontainer

# Delete container
az container delete \
  --resource-group myRG \
  --name mycontainer --yes

# --- Azure Container Apps ---

# Create a Container Apps environment
az containerapp env create \
  --name myEnv \
  --resource-group myRG \
  --location eastus

# Create a container app
az containerapp create \
  --name myapp \
  --resource-group myRG \
  --environment myEnv \
  --image mcr.microsoft.com/azuredocs/containerapps-helloworld:latest \
  --target-port 80 \
  --ingress external \
  --min-replicas 0 \
  --max-replicas 10
```

### PowerShell

```powershell
# --- Azure Container Registry ---
New-AzContainerRegistry -ResourceGroupName "myRG" `
  -Name "myregistry" -Sku Standard -Location EastUS

Connect-AzContainerRegistry -Name "myregistry"

# --- Azure Container Instances ---
$port = New-AzContainerInstancePortObject -Port 80 -Protocol TCP
$container = New-AzContainerInstanceObject `
  -Name "mycontainer" `
  -Image "mcr.microsoft.com/azuredocs/aci-helloworld" `
  -Port @($port)

New-AzContainerGroup -ResourceGroupName "myRG" `
  -Name "myContainerGroup" -Location EastUS `
  -Container $container -OsType Linux `
  -IPAddressDnsNameLabel "my-aci-app" `
  -IpAddressType Public -IPAddressPorts @($port)

# Get container group status
Get-AzContainerGroup -ResourceGroupName "myRG" `
  -Name "myContainerGroup"
```

---

## Confusion Matrix

| Feature | ACI | Container Apps |
| --------------------------- | ----------------------------------- | ------------------------------------ |
| Abstraction level | Lower-level building block | Higher-level serverless platform |
| Auto-scaling | NO (manual) | YES (KEDA-powered) |
| Scale to zero | NO | YES |
| Load balancing | NO (manual) | YES (built-in) |
| Traffic splitting | NO | YES (revisions) |
| Service discovery | NO | YES (DNS-based) |
| Certificates / TLS | NO (manual) | YES (built-in) |
| Multi-container | YES (container group) | YES (sidecars) |
| Dapr integration | NO | YES |
| Kubernetes API access | NO | NO (use AKS) |
| Best for | Short tasks, burst, simple workloads | Microservices, APIs, event-driven |
| Billing | Per-second (CPU + memory) | Per-second (can scale to 0 = free) |

| Feature | ACR Basic | ACR Standard | ACR Premium |
| ------------------- | --------- | ------------ | ----------- |
| Geo-replication | NO | NO | YES |
| Private endpoints | NO | NO | YES |
| Content trust | NO | NO | YES |
| Customer-managed keys | NO | NO | YES |
| Webhooks | 2 | 10 | 500 |
| Anonymous pull | NO | YES | YES |

| Restart Policy | Behavior | Use When |
| -------------- | ----------------------- | ---------------------- |
| Always | Always restart (default) | Web servers, APIs |
| OnFailure | Restart on error | Batch jobs |
| Never | Run once only | One-time tasks/scripts |

---

## Decision Rules

| If... | Then... |
| ----------------------------------------------------------- | ---------------------------------------------------------------- |
| Need a private Docker registry in Azure | Use **ACR** |
| Need geo-replication for container images | Use **ACR Premium** |
| Need private endpoints for registry access | Use **ACR Premium** |
| Simple, short-lived container without orchestration | Use **ACI** |
| Need auto-scaling microservices without managing K8s | Use **Container Apps** |
| Need scale to zero (pay nothing when idle) | Use **Container Apps** |
| Need traffic splitting / blue-green deployments | Use **Container Apps** (revisions) |
| Need full Kubernetes API access | Use **AKS** (not ACI or Container Apps) |
| Need to run multi-container group on Windows | NOT supported — use single container on ACI for Windows |
| Need to change ACI CPU/memory allocation | **Delete and recreate** the container group |
| Container App won't scale back up from zero | Check: ingress must be enabled; or set `minReplicas >= 1` |

---

## 5 Common Scenario Patterns

**Scenario 1: Store images privately for production deployment**

> "Contoso wants to store container images in a private registry with geo-replication across US and Europe."

- Create **ACR Premium** registry
- Enable **geo-replication** to both US and Europe regions
- Use **private endpoints** for secure access from VNets
- Deploy to ACI or Container Apps from the registry

**Scenario 2: Run a batch processing job**

> "A nightly data processing job runs a container that should execute once and stop."

- Use **ACI** with restart policy = **Never**
- Set appropriate CPU/memory (e.g., 2 CPU, 4 GB)
- Container runs, processes data, then terminates
- Billed only for execution time (per-second)

**Scenario 3: Deploy a scalable microservice API**

> "Contoso needs an API that auto-scales based on HTTP traffic and costs nothing when idle."

- Use **Container Apps** with external ingress enabled
- Configure HTTP scale rule: e.g., 100 concurrent requests per replica
- Set min replicas = 0, max replicas = 20
- App scales to zero when no traffic → no charges

**Scenario 4: Multi-container sidecar pattern**

> "An application container needs a logging sidecar container that ships logs to a central service."

- Use **ACI container group** with 2 containers (Linux)
- App container + logging container share the same network (localhost)
- Both share an Azure File share volume for log files

**Scenario 5: Upgrade container app with zero downtime**

> "Deploy a new version of a Container App without interrupting users."

- Container Apps creates a new **revision** with the updated image
- Configure **traffic splitting**: 90% to old revision, 10% to new
- Gradually shift traffic (canary deployment)
- Once validated, route 100% to new revision

---

## Exam Traps

> **TRAP 1:** ACI **multi-container groups are Linux only** — Windows containers support only a single container per group.

> **TRAP 2:** ACI restart policy default is **Always** — if you deploy a one-time task without setting it to Never, the container will keep restarting.

> **TRAP 3:** To change ACI resources (CPU, memory, OS type, restart policy), you must **delete and recreate** the container group — these cannot be updated in-place.

> **TRAP 4:** Container Apps can scale to zero, but apps that scale based on **CPU or memory cannot scale to zero** — only HTTP, TCP, and event-driven triggers support scale to zero.

> **TRAP 5:** ACR **geo-replication is Premium only** — Basic and Standard do not support it. You must remove geo-replications before downgrading from Premium.

> **TRAP 6:** ACR registry name must be **globally unique** and contain only **alphanumeric characters** (5–50 chars, no dashes or special characters).

> **TRAP 7:** Container Apps does **NOT** expose the Kubernetes API — if you need direct K8s API access, use AKS instead.

> **TRAP 8:** ACI has **no built-in auto-scaling** — to scale, you must create additional container groups manually or via automation. Container Apps has built-in KEDA autoscaling.

> **TRAP 9:** Container Apps **adding or editing a scale rule creates a new revision** — revisions are immutable snapshots.

> **TRAP 10:** If Container Apps ingress is **disabled** and no custom scale rule or `minReplicas` is set, the app scales to zero and **cannot start back up** — always set `minReplicas >= 1` or enable ingress.

---

## Rapid-Fire Q&A

**Q: What are the three ACR SKUs?**
A: Basic, Standard, Premium

**Q: Which ACR SKU supports geo-replication?**
A: Premium only

**Q: Which ACR SKU supports private endpoints?**
A: Premium only

**Q: What is the ACR login server format?**
A: `<registryname>.azurecr.io`

**Q: What is the default ACI restart policy?**
A: Always

**Q: Can ACI multi-container groups run Windows?**
A: No — multi-container groups are Linux only

**Q: What is the minimum ACI resource allocation?**
A: 1 CPU + 1 GB memory per container group

**Q: What is the max ACI resource allocation (standard)?**
A: 31 CPU + 240 GB memory

**Q: Can you update ACI CPU/memory in-place?**
A: No — must delete and recreate the container group

**Q: What powers Container Apps autoscaling?**
A: KEDA (Kubernetes Event-driven Autoscaling)

**Q: Can Container Apps scale to zero?**
A: Yes — except for CPU/memory-based scale triggers

**Q: What is the default max replicas for Container Apps?**
A: 10 (configurable up to 1,000)

**Q: What is a Container Apps revision?**
A: An immutable snapshot of a container app version

**Q: Does Container Apps expose the Kubernetes API?**
A: No — use AKS if you need direct K8s API access

**Q: What happens if you edit a Container Apps scale rule?**
A: A new revision is created

**Q: What is the difference between ACI and Container Apps?**
A: ACI = lower-level building block (no auto-scale, no LB). Container Apps = higher-level serverless (auto-scale, traffic splitting, service discovery)

**Q: How do you build a container image without local Docker?**
A: Use `az acr build` (ACR Tasks builds in the cloud)

**Q: What volume types does ACI support?**
A: Azure File share, secret, empty directory, git repo

**Q: What is the ACI storage limit per container group?**
A: 50 GB
