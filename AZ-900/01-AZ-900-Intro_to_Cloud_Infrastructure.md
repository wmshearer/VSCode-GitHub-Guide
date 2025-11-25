# AZ-900 — Deploy Azure Container Instances & Core Cloud Concepts

## Azure Container Instances (ACI)

Azure Container Instances allow you to run containers *without managing servers or VMs*.  
ACI is ideal for **simple, isolated container workloads** where you don’t need orchestration (unlike AKS).

### When to Use ACI
- Quick deployment of containers
- Testing or dev workloads
- Short-lived batch jobs
- Public-facing lightweight apps

---

## Creating an Azure Container Instance (Lab Summary)

### Steps to Create a Container Instance
1. Sign in to the Azure Portal  
   `https://portal.azure.com`
2. Navigate to **Container Instances** → **+ Create**
3. Configure **Basics**:
   - Subscription: *Default*
   - Resource Group: *Default*
   - Container name: `mycontainer`
   - Region: `(US) East US`
   - Image source: *Docker Hub or Other*
   - Image: `mcr.microsoft.com/azuredocs/aci-helloworld`
   - OS Type: *Linux*

### Networking Configuration
- DNS name label must be **globally unique**  
  Example: `mycontainerdnsxxxxx`

Container URL format:  
```
mycontainerdnsxxxxx.region.azurecontainer.io
```

### Monitoring
- Disable **Container instance logs** (not needed here)

### Deployment Verification
- After deployment, go to the resource
- Ensure **Status = Running**
- Copy the **FQDN** and paste in a browser
- You should see:

> "Welcome to Azure Container Instances!"

---

## Key Azure Cloud Concepts (Exam-Focused)

### What is Cloud Computing?
Cloud computing is:

> **The delivery of computing services over the internet (“the cloud”).**

This includes:
- Servers
- Storage
- Networking
- Databases
- Analytics
- Applications

Cloud computing is:
- On-demand
- Pay-as-you-go
- Elastic & scalable

---

## Cloud Models (Critical AZ-900 Topic)

### Public Cloud
- Services delivered over the internet
- Shared physical hardware
- Accessible to anyone with a subscription
- Examples: Azure, AWS, GCP

### Private Cloud
- Cloud resources dedicated to *one organization*
- Hosted on-premises or by a third-party

### Hybrid Cloud
**Correct answer to the quiz question: Hybrid cloud**

Hybrid cloud =  
> Combination of public + private cloud, working together as one.

Used when:
- You have regulatory or data residency needs  
- Want to keep sensitive workloads in private datacenter  
- Want flexibility and scalability from cloud

### Multicloud
- Using **multiple cloud providers** (Azure + AWS, etc.)
- Not the same as hybrid

---

## Shared Responsibility Model

### Overview
Defines **who manages what** between:
- Azure (cloud provider)
- Customer (you)

Responsibility depends on cloud service type.

### Service Models (Ordered from MOST customer responsibility → LEAST)

#### 1. Infrastructure as a Service (IaaS)
**Customer has the MOST responsibility.**

You manage:
- OS
- Software
- Runtime
- Apps
- Data
- Virtual network

Azure manages:
- Physical hosts
- Networking
- Storage

Examples:
- Azure VM
- Azure Virtual Networks
- Azure Storage disks

#### 2. Platform as a Service (PaaS)
Azure manages more:

Azure manages:
- OS
- Runtime
- Security patches
- Middleware

You manage:
- Apps
- Data

Examples:
- Azure App Service
- Azure SQL Database

#### 3. Software as a Service (SaaS)
**Customer has the LEAST responsibility.**

Azure manages:
- Everything  
You only use the software.

Examples:
- Microsoft 365
- Dynamics 365

---

## Module Assessment — Correct Answers

### 1. What is cloud computing?
✔ **Delivery of computing services over the internet.**

### 2. Which cloud model uses some datacenters for public cloud customers and some for a single customer?
✔ **Hybrid cloud**

### 3. Which cloud service type places the most responsibility on the customer?
✔ **Infrastructure as a Service (IaaS)**

---

## Memory Boost (Exam Tips)

### Cloud Models (One-Liners)
- **Public Cloud** → “Azure for everyone”
- **Private Cloud** → “Your datacenter, cloud-style”
- **Hybrid Cloud** → “Best of both worlds”
- **Multicloud** → “More than one provider”

### Service Models Trick
Use the acronym:  
**I→P→S** = **I Manage More → P Manage Medium → S Manage Least**

- **IaaS =** You manage almost everything  
- **PaaS =** You manage apps & data  
- **SaaS =** You just use the app  

### ACI Exam Tip
ACI shows up as:
- “Run containers *without servers*”
- “Serverless containers”
- “Quick and simple container hosting”
- “No orchestration”

# AZ-900 — Core Cloud Benefits: Availability, Scalability, Reliability, Predictability, Security, and Manageability

## High Availability & Scalability

### High Availability
High availability ensures systems remain accessible even when failures or disruptions occur.

Key points:
- Focuses on **uptime**.
- Azure provides **service-level agreements (SLAs)** guaranteeing specific availability levels.
- Designed to withstand failures such as:
  - Hardware issues  
  - Network outages  
  - Datacenter disruptions
- Azure services automatically handle redundancy and failover.

**Exam trigger phrase:**  
“Ensures resources are available when needed.”

---

### Scalability
Scalability = ability to **increase or decrease resources** to meet changing demand.

Benefits:
- Prevents systems from being overwhelmed during traffic spikes.
- Reduces cost when demand decreases (pay only for what you use).
- Supports unpredictable or fluctuating demand patterns.

Two types:

#### Vertical Scaling (Scaling Up / Down)
- Increase or decrease power of a single resource.
- Example:
  - Add more CPU/RAM to a VM → scale up  
  - Reduce CPU/RAM → scale down

#### Horizontal Scaling (Scaling Out / In)
- Add or remove **multiple** resource instances.
- Example:
  - Add more VMs or containers → scale out  
  - Remove instances → scale in
- Can be **manual or automatic (autoscaling)**.

**Exam trigger phrase:**  
“Add/remove instances automatically” = horizontal scaling.

---

## Reliability & Predictability

### Reliability
Reliability = ability of a system to **recover from failures** and continue functioning.

Key points:
- Cloud uses **decentralized global infrastructure**.
- Resources can be deployed across many regions.
- Applications can automatically shift to different regions if one fails.
- Part of the **Azure Well-Architected Framework**.

**Exam keywords:**  
“Resilient infrastructure”  
“Recover from failures”  
“Continue to function”

---

### Predictability
Predictability = consistent **performance** and **cost** expectations.

Two types:

#### Performance Predictability
- Knowing how resources will behave under load.
- Powered by:
  - Autoscaling  
  - High availability  
  - Load balancing  
  - Resource redundancy  
- Ensures consistent customer experience during peaks.

#### Cost Predictability
- Ability to forecast cloud spending.
- Tools:
  - **Azure Pricing Calculator**
  - **TCO Calculator**
  - Azure Cost Management
- Cloud provides real-time monitoring and analytics.
- Helps avoid overspending.

---

## Security & Governance

### Governance Benefits
Cloud governance supports:
- Compliance with standards & regulations.
- Enforced consistency using templates:
  - ARM templates
  - Policy definitions
  - Blueprints
- Ability to update policies across all deployed resources.
- Cloud auditing identifies non-compliant resources.

Governance enables:
- Tagging  
- Access control  
- Standardized configurations  
- Organization-wide policy enforcement  

---

### Security Benefits
Cloud security includes:
- Over-the-internet delivery optimized for DDoS protection.
- Built-in protections and global threat intelligence.
- Depending on your preference, choose:
  - **IaaS:** maximum customer control  
  - **PaaS:** automated patching & maintenance  
  - **SaaS:** least responsibility (provider-managed)

Cloud providers handle:
- Physical datacenter security  
- Network protection  
- Platform patching (for PaaS/SaaS)

**Exam tip:**  
DDoS mitigation is a **cloud platform** strength.

---

## Manageability

Manageability consists of two perspectives:

### 1. Management **of** the Cloud (managing cloud resources)
Cloud enables:
- Automatic scaling based on need.
- Deployment from **preconfigured templates**.
- Automated replacement of failing resources.
- Real-time monitoring & alerts.

You can centrally manage:
- VMs  
- Storage  
- Networks  
- Databases  
- Applications  

---

### 2. Management **in** the Cloud (how you manage the environment)
You can manage Azure using:
- Azure Portal (GUI)
- Azure CLI
- Azure PowerShell
- REST APIs
- SDKs

Cloud management is flexible across:
- Web  
- Command line  
- Automation  
- Scripts  

---

## Module Assessment — Correct Answers

### 1. Which type of scaling involves adding or removing resources like VMs or containers?
✔ **Horizontal scaling**

### 2. What is the ability of a system to recover from failures and continue to function?
✔ **Reliability**

# AZ-900 — Cloud Service Types (IaaS, PaaS, SaaS)

Cloud services fall into three categories that differ in how responsibilities are shared between you and the cloud provider.  
Understanding these differences is **critical** for AZ-900.

---

## Infrastructure as a Service (IaaS)

### What IaaS Is
IaaS is the **most flexible** cloud service model.  
You rent fundamental compute, storage, and network resources—but manage everything above the hardware layer.

Think of it as:
> “You manage the OS and everything above it. Azure manages the physical datacenter.”

### Responsibilities (Shared Responsibility Model)
Azure manages:
- Physical hardware  
- Datacenter security  
- Physical networking  
- Power, cooling, environment  

You manage:
- OS installation, configuration, patching  
- VMs, storage, networking configuration  
- Middleware  
- Applications  
- Data  
- Security for OS and apps  

### Ideal Scenarios for IaaS
- **Lift-and-shift migration:** Move on-prem VMs directly to Azure with minimal changes.  
- **Development & test:** Quickly clone or reset full environments.  
- Running custom workloads requiring full OS control.

### Exam trigger words
- “Full control”  
- “Lift and shift”  
- “You manage OS and applications”  
- “Most customer responsibility”  

---

## Platform as a Service (PaaS)

### What PaaS Is
PaaS provides an **application platform** managed by the cloud provider.

It includes:
- OS  
- Runtime  
- Middleware  
- Development tools  
- Database engines  

You only manage:
- Your applications  
- Your data  

Think of it as:
> “Azure manages the platform. You build and deploy your app.”

### Responsibilities
Azure manages:
- Physical infrastructure  
- OS patching  
- Runtime/framework updates  
- DB engine updates  
- Middleware  
- Security patching  

You manage:
- Application code  
- Data stored in services  
- App-level configuration  

### Ideal Scenarios for PaaS
- **Application development frameworks**  
  (Azure App Service, Azure Functions, Logic Apps)
- **Analytics and BI**  
  (Azure Synapse, Azure Databricks)
- Apps that need built-in:
  - scalability  
  - high-availability  
  - multi-tenancy  

### Exam trigger words
- “Managed platform”  
- “No OS patching required”  
- “Focus on app development”  

---

## Software as a Service (SaaS)

### What SaaS Is
SaaS delivers fully managed **applications** to you.

Think of it as:
> “You just use the software. Everything else is handled.”

Examples:
- Microsoft 365  
- Dynamics 365  
- Outlook.com  
- Teams  
- Gmail  
- Salesforce  

### Responsibilities
Azure/the provider manages:
- Datacenters  
- Hardware  
- OS  
- Application code  
- Updates & patches  
- Security  

You manage:
- Your data  
- User access and identity  
- Device security  

### Ideal Scenarios for SaaS
- Email & messaging  
- Finance and expense tracking  
- Productivity apps  
- CRM/ERP tools  

### Exam trigger words
- “Fully managed application”  
- “Least customer responsibility”  
- “Turnkey solution”  

---

# Summary Table (High-Value Exam View)

| Category | Who Manages More? | Customer Responsibilities | Common Use Cases |
|---------|-------------------|---------------------------|------------------|
| **IaaS** | **Customer** | OS, apps, data, networking | Lift-and-shift, dev/test |
| **PaaS** | Shared | App + data only | Web apps, APIs, databases |
| **SaaS** | **Provider** | Data + users | M365, CRM, finance apps |

---

# Module Assessment — Correct Answers

### 1. Which cloud service type is most suited for lift-and-shift migrations?
✔ **Infrastructure as a Service (IaaS)**

### 2. Finance and Expense tracking is typically which service type?
✔ **Software as a Service (SaaS)**
