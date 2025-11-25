# AZ-900 — Management & Governance (Costs, Tools, Tags)

## Factors That Affect Costs in Azure

### CapEx vs OpEx

- **On-prem**: Capital Expenditure (CapEx) — buy hardware, build datacenter.
- **Azure**: Operational Expenditure (OpEx) — rent compute, storage, networking as needed.

### Main Cost Factors

1. **Resource Type**
   - Different services and configurations = different prices.
   - Examples:
     - **Storage account**:
       - Type (Blob, Files, Tables, Queues)
       - Performance tier (Standard/Premium)
       - Access tier (Hot/Cool/Cold/Archive where applicable)
       - Redundancy (LRS, ZRS, GRS, GZRS, etc.)
       - Region
     - **Virtual machine (VM)**:
       - Size (vCPUs, RAM)
       - OS/license (Windows vs Linux)
       - Attached disks (HDD/SSD, size, performance)
       - Network (public IP, bandwidth)
       - Region

2. **Consumption**
   - **Pay-as-you-go**:
     - Pay for what you *actually* use each billing cycle.
   - **Reserved capacity**:
     - Commit to 1- or 3-year usage for many services (VMs, DBs, storage).
     - Can get significant discounts (up to ~72%).
     - If usage exceeds reservation → extra usage billed pay-as-you-go.

3. **Maintenance (Housekeeping)**
   - Extra resources created with a VM:
     - NICs, disks, public IPs, NSGs, etc.
   - Deleting the VM does **not always** delete everything.
   - Regularly clean up:
     - Orphaned disks
     - Idle public IPs
     - Unused resource groups
   - Good resource group organization makes cleanup easier.

4. **Geography & Network Traffic**
   - Regions differ in cost (power, labor, taxes, fees).
   - **Data transfer (bandwidth)**:
     - Inbound (data into Azure) often **free**.
     - Outbound (data leaving Azure) charged by **billing zone** and volume.
     - Cross-region / cross-geography transfers generally cost more.

5. **Subscription Type**
   - Some subscription offers include **free usage** or **credits**:
     - Free trial: 12 months of certain free services + 30-day credit + “always free” services.
     - Student/free accounts have their own allowances.
   - Once limits/credits are exceeded → standard billing rates.

6. **Azure Marketplace**
   - Third-party solutions running on Azure:
     - Prebuilt VMs with software, firewalls, backup solutions, etc.
   - You may pay:
     - Azure infrastructure cost **plus**
     - Vendor software or service fees (their billing model).
   - All Marketplace solutions must meet Azure certification policies.

---

## Azure Pricing Calculator

### Purpose

- Public web tool to **estimate** the cost of Azure services **before** deployment.
- Lets you:
  - Add individual services (VMs, DBs, Storage, Networking, etc.)
  - Adjust configuration (region, size, redundancy, hours/month, data transfer, etc.)
  - See estimated monthly cost for your scenario or use **example architectures**.

> Important: It is **only** an estimator.  
> Nothing is deployed, and you aren’t billed for anything configured in the calculator.

### Typical Use

- Plan cost for:
  - New workloads and migrations
  - “What if” scenarios (different regions, VM sizes, tiers)
- Supports:
  - Export to Excel
  - Save estimates
  - Share a link with others

---

## Microsoft Cost Management

### What is Cost Management?

- Azure service to **monitor and control** actual spending.
- Key capabilities:
  - **Cost analysis**: visualize and break down costs (by subscription, resource group, resource, region, service, etc.)
  - **Budgets**: set spend limits and thresholds.
  - **Alerts**: get notified when usage or spend crosses thresholds.

### Cost Analysis

- Provides charts & graphs of:
  - Current and historical spend
  - Trends vs budget (monthly/quarterly/yearly)
  - Spend by region, service, subscription, RG, etc.
- Helps identify:
  - Unexpected spikes
  - Expensive services
  - Optimization opportunities

### Cost Alerts

Three main alert types:

1. **Budget alerts**
   - Trigger when actual or forecasted spending **reaches a percentage** of your defined budget (e.g., 50%, 75%, 100%).
   - Budget can be scoped to:
     - Subscription, resource group, service type, etc.
   - Can send emails and/or trigger automation to act on resources.

2. **Credit alerts**
   - For orgs with **monetary commitments** (Enterprise Agreements).
   - Trigger at 90% and 100% of Azure credit usage.

3. **Department spending quota alerts**
   - For EA departments with set quotas.
   - Trigger emails at defined percentages (e.g., 50%, 75%).

### Budgets

- Define a **spending limit/target** for:
  - Subscription, resource group, service type, etc.
- Alerts when threshold is reached.
- Advanced:
  - Integrate with automation (e.g., via Logic Apps) to:
    - Stop/scale resources
    - Change configurations
    - Send internal notifications.

---

## Tags (Metadata for Organization)

### What Are Tags?

- **Key–value pairs** attached to **resources, resource groups, or subscriptions**.
- Example:
  - `Environment = Prod`
  - `Owner = JohnDoe`
  - `CostCenter = FIN-001`

### Why Use Tags?

1. **Resource Management**
   - Find and manage resources by:
     - Workload
     - Environment (Dev/Test/Prod)
     - Business unit
     - Owner

2. **Cost Management & Optimization**
   - Group costs by:
     - Department
     - Project
     - AppName
     - CostCenter
   - Great for chargeback/showback and budget tracking.

3. **Operations Management**
   - Tag resources by **criticality**:
     - `Impact = Mission-critical / High / Low`
   - Helps design and report on **SLAs** and priorities.

4. **Security**
   - Classify data/resources by sensitivity:
     - `DataClass = Public / Internal / Confidential`
   - Use tags to identify where stronger controls are required.

5. **Governance & Compliance**
   - Track resources that must comply with specific standards:
     - `Compliance = ISO27001 / HIPAA / PCI`
   - Use tags to enforce internal standards and policies.

6. **Workload Optimization & Automation**
   - Tag by application / workload:
     - `AppName = InventoryAPI`
   - Automation tools (Azure DevOps, scripts) can:
     - Filter by tags
     - Apply updates, scale rules, or maintenance to specific workloads.

### Managing Tags

- Can be created/edited via:
  - Azure Portal
  - PowerShell
  - Azure CLI
  - ARM/Bicep templates
  - REST API

- **Azure Policy** can:
  - Enforce required tags on new resources.
  - Automatically add or inherit tags.
  - Audit missing/incorrect tags.

- Tags **do not inherit** automatically:
  - Resources don’t auto-inherit tags from their resource group/subscription.
  - Lets you use different tag schemes at different levels if needed.

### Sample Tagging Scheme

| Name        | Value Example                   | Use                                         |
|------------|----------------------------------|---------------------------------------------|
| `AppName`  | InventoryService                 | Group resources by application/workload     |
| `CostCenter` | FIN-001                         | Internal billing/tracking                   |
| `Owner`    | Jane.Doe                         | Contact point / business owner              |
| `Environment` | Prod / Dev / Test             | Lifecycle/environment management            |
| `Impact`   | Mission-critical / High / Low    | SLA and priority classification             |

---

## Module Assessment — Correct Answers

1. **What Azure feature can help stay organized and track usage based on metadata associated with resources?**  
   ✔ **Tags**

2. **Which tool helps estimate the cost of deploying Azure services before implementation?**  
   ✔ **Azure Pricing Calculator**

# AZ-900 — Management & Governance (Purview, Policy, Locks, Service Trust Portal)

## Microsoft Purview — Purpose

Microsoft Purview is Microsoft’s **unified data governance, risk, and compliance platform**.  
Its job is to give you a **single view of your entire data estate**, no matter where your data lives—Azure, on-prem, AWS, SQL, SaaS apps like M365, etc.

### What Purview Provides

**1. Automated Data Discovery**  
- Scans your environment and finds data sources automatically.

**2. Sensitive Data Classification**  
- Detects sensitive info: credit cards, SSNs, names, financial info.

**3. End-to-End Data Lineage**  
- Shows where data came from, how it moves, and where it goes.

Purview solutions fall into two big categories:

---

### **Purview Risk & Compliance**

Built on Microsoft 365 services (Exchange, Teams, OneDrive, etc.):

- Protect sensitive data across all locations  
- Identify risks + meet regulatory compliance  
- Provide readiness for compliance standards (GDPR, HIPAA, ISO)

---

### **Purview Unified Data Governance**

For Azure, SQL, on-prem, and multicloud (AWS S3, etc.):

- Create a complete map of your data estate  
- Locate sensitive data  
- Provide a secure catalog for data consumers  
- Offer insights on how data is accessed  
- Manage access safely and at scale  

---

## Azure Policy — Purpose

Azure Policy is used to **enforce standards and evaluate compliance** across Azure resources.

Azure Policy:

- Creates rules that control what can or can’t be deployed  
- Audits existing resources for compliance  
- Can auto-remediate violations  
- Prevents creation of non-compliant resources  

Policies can be applied at:

- Resource  
- Resource group  
- Subscription  
- Management group  

Policies **inherit downward**, meaning high-level policies apply automatically to lower levels.

### Policy Example  
- Only allow VMs of a certain size  
- Require specific tags (AppName, Owner, CostCenter)  
- Enforce storage accounts to use HTTPS  
- Block resources in unauthorized regions

### Policy Initiatives  
Initiatives = groups of related policies for a broader goal.

Example: **Enable Monitoring in Azure Security Center** (over 100 policies included).

---

## Resource Locks — Purpose

Resource locks prevent accidental deletion or modification of important assets.

Lock types:

### **Delete**
- Resource **cannot be deleted**
- Still can read/modify it

### **ReadOnly**
- Resource **cannot be changed or deleted**
- Only read access is allowed

Locks apply at:

- Resource level  
- Resource group  
- Subscription  

Locks inherit downward—locking a resource group locks everything inside it.

Even Owners must remove the lock before making the restricted change.

---

## Service Trust Portal — Purpose

The Microsoft **Service Trust Portal (STP)** provides documentation and resources about:

- Security
- Compliance
- Privacy practices  
- Audit reports (SOC, ISO, FedRAMP)
- Compliance guides and control mappings

You can access it at: **https://servicetrust.microsoft.com**

Many documents require:

- Signing in with your Microsoft cloud account  
- Accepting an NDA

### Main Menu Sections

- **Service Trust Portal** — return home  
- **My Library** — bookmark documents & get update notifications  
- **All Documents** — browse all compliance/security documents  

---

## Module Assessment — Correct Answers

### **1. How can you prevent creation of non-compliant resources?**  
✔ **Azure Policy**

### **2. What’s the best way to prevent inadvertent deletion of a resource?**  
✔ **Azure resource locks**

# AZ-900 — Tools for Interacting with Azure  
_Last section of the exam — Management & Governance_

---

## Azure Portal  
The **Azure Portal** is a **browser-based GUI** for managing everything in Azure.

### What you can do with it:
- Build, deploy, manage, and monitor cloud resources  
- Create **custom dashboards** for visibility  
- Use accessibility features and global redundancy (portal runs in every Azure datacenter)  
- No downtime—updates continuously  

Best for:
- Visual management  
- Rapid exploration  
- Learning the platform  

---

## Azure Cloud Shell  
**Cloud Shell** is a **browser-based** command-line environment.

### Key features:
- Runs directly in the browser  
- No installation required  
- Automatically authenticated to your Azure account  
- Supports:
  - **PowerShell**
  - **Azure CLI (bash)**

Great for:
- Administration without installing tools  
- Running CLI commands from any device  

---

## Azure PowerShell  
A **PowerShell module** that uses cmdlets to manage Azure via Azure’s REST API.

### Uses:
- Automating admin tasks  
- Running scripts  
- Deploying full environments programmatically

Runs on:
- Windows  
- macOS  
- Linux  
- Azure Cloud Shell

Best for:
- Admins & engineers comfortable with PowerShell  
- Infrastructure automation  

---

## Azure CLI  
Command-line tool that manages Azure using **bash-style syntax**.

### Features:
- Identical functionality to Azure PowerShell  
- Runs locally or in Cloud Shell  
- Scriptable, automation-friendly

Best for:
- Developers & Linux/Bash users  
- Cross-platform scripting  

**CLI vs PowerShell?**  
Both can do the same things.  
Choose the language you prefer.

---

# Azure Arc — Purpose  
**Azure Arc extends Azure management to ANY environment**:  
- On-premises  
- AWS  
- GCP  
- Edge environments  

### What Arc Enables:
- Project non-Azure resources into Azure Resource Manager  
- Apply Azure policies, monitoring, and RBAC to external resources  
- Manage:
  - Servers  
  - SQL Servers  
  - Kubernetes clusters  
  - Databases  
  - VMs (preview)

Arc gives you **a single control plane** for hybrid or multi-cloud.

---

# Azure Resource Manager (ARM) & ARM Templates  

Azure Resource Manager = **the backend engine** that processes ALL Azure actions.

When you deploy anything:
- Portal / CLI / PowerShell → ARM  
- ARM authenticates & authorizes  
- ARM deploys and monitors the resource  

### ARM Benefits:
- Group deployments  
- Consistent, repeatable results  
- Dependency ordering  
- Built-in RBAC  
- Tag support  
- Unified management layer  

---

# Infrastructure as Code (IaC)

Azure supports IaC through:

## **ARM Templates**
- Written in **JSON**
- Declarative (describe *what* to deploy, not *how*)  
- Allows:
  - Repeatable deployments  
  - Parallel creation  
  - Dependencies  
  - Modular templates  
  - Extensibility (PowerShell/Bash scripting)

---

## **Bicep**  
A modern, simplified declarative language that compiles into ARM templates.

### Why Bicep?
- Much cleaner and shorter than JSON  
- Full support for all Azure resource types  
- Modules for reusability  
- Idempotent deployments  
- Better readability

**Bicep = preferred IaC tool for Azure today**

---

# Module Assessment — Correct Answers

### **1. What service helps you manage your Azure, on-premises, and multicloud environments?**  
✔ **Azure Arc**

### **2. What two components could you use to implement an “infrastructure as code” deployment?**  
✔ **Bicep and ARM Templates**

# AZ-900 — Azure Advisor, Service Health, and Azure Monitor  
_Final section of Management & Governance_

---

# Azure Advisor — Purpose  
**Azure Advisor** is a built-in recommendation engine that analyzes your resources and tells you how to:

- Improve **reliability (high availability)**
- Improve **security**
- Improve **performance**
- Improve **operational excellence**
- Reduce **cost**

Advisor evaluates your actual environment and produces **personalized recommendations**.  
You can view them in the portal, via API, or get email alerts.

### 5 Recommendation Categories
| Category | Purpose |
|---------|---------|
| **Reliability** | Improve uptime / business continuity |
| **Security** | Identify threats & vulnerabilities |
| **Performance** | Improve app speed and responsiveness |
| **Operational Excellence** | Improve processes, monitoring, deployment practices |
| **Cost** | Reduce unnecessary Azure spend |

---

# Azure Service Health — Purpose  
Azure Service Health gives you a **full picture of Azure availability**, from global status all the way down to individual resources.

### Azure Service Health = **3 services combined**

## 1. **Azure Status (Global View)**
- Public page  
- Shows major outages across **all Azure regions & services**  
- High-level, not personalized  

## 2. **Service Health (Your Services & Regions)**
- Personalized dashboard  
- Shows:
  - Outages  
  - Planned maintenance  
  - Health advisories  
- BEST place to check if *your workloads* are affected  
- Can trigger alerts  

## 3. **Resource Health (Individual Resource View)**
Shows health of a **specific resource**, like a VM or App Service:
- Healthy  
- Degraded  
- Unavailable  
- Unknown  

Useful for diagnosing:
- VM reboots  
- Connectivity issues  
- Service interruptions  

---

# Azure Monitor — Purpose  
**Azure Monitor** is Azure’s unified platform for collecting, analyzing, and acting on telemetry across:

- Applications  
- Infrastructure  
- Networks  
- Hybrid & multicloud setups  

### What Azure Monitor Collects:
- Logs  
- Metrics  
- Diagnostics  
- Performance data  
- Alerts  

### Core Components

---

## **Azure Log Analytics**
- Query and analyze logs and metrics  
- Uses **Kusto Query Language (KQL)**  
- Great for investigations, dashboards, trends  

---

## **Azure Monitor Alerts**
- Triggered on *metrics* or *log events*  
- Example: VM CPU > 80%  
- Alerts use **Action Groups** for:
  - Email  
  - SMS  
  - Webhooks  
  - Automation (Functions/Logic Apps)

---

## **Application Insights**
Monitors application performance and user behavior.

Tracks:
- Request rates and failures  
- Page load times  
- Dependencies (SQL, APIs, etc.)  
- Exceptions  
- Browser & AJAX data  
- Synthetic testing (availability monitoring)

Works for:
- Azure apps  
- On-prem apps  
- Apps in AWS/GCP  

---

# Module Assessment Answers

### **1. Which is NOT one of the Azure Advisor recommendation categories?**
✔ **Capacity**  
(Reliability, Security, Cost, Performance, Operational Excellence ARE categories.)

---

### **2. You receive an email about a VM outage in an Azure region. Which part of Azure Service Health tells you if *your* app is impacted?**
✔ **Resource Health**

