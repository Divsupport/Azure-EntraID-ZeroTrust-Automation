

<h1 align="center">🔐 Enterprise IAM Governance: Scoped RBAC, Administrative Units, and Delegated Resource Ownership in Microsoft Entra ID</h1>

<h3 align="center">
Built by <span style="color:#0078D4;">Divine Oguamanam</span>
</h3>

---

<h2>📖 Project Overview</h2>

In enterprise environments, over-provisioning administrative privileges creates a massive attack surface and increases the blast radius of credential compromise. This project demonstrates a production-grade Identity and Access Management (IAM) governance model implemented within **NexusCorp Global** for its **New York Regional Branch**.

The architecture establishes secure identity segregation using **Microsoft Entra ID Administrative Units (AUs)**, scoped **Role-Based Access Control (RBAC)**, and **delegated business resource ownership**. By restricting regional administrators' control to localized containers and offloading everyday communication group management directly to departmental heads, this deployment successfully enforces the **Principle of Least Privilege (PoLP)** and decouples control-plane configuration from daily data-plane collaboration.

<h3>🔑 Key Security Architecture Pillars</h3>

- **Least Privilege Access (PoLP):** Eliminating directory-wide standing privileges for regional staff.
- **Scoped Administration Boundaries:** Utilizing Administrative Units to segment tenant control lines.
- **Delegated Resource Ownership:** Offloading roster management to department leaders using object-level ownership.
- **Separation of Duties (SoD):** Isolating core infrastructure security groups from collaborative M365 environments.
- **Identity Security Validation:** Executing an adversarial-style verification matrix to prove boundary enforcement.

<br />

<h2>🛠️ Technologies, Frameworks, and Tools</h2>

- **Directory Service Platform:** Microsoft Entra ID (Azure Active Directory)
- **Control Interface:** Microsoft Azure Portal & Entra Admin Center (`entra.microsoft.com`)
- **Access Control Engines:** Role-Based Access Control (RBAC) & Scoped Directory Roles
- **Identity Container Staging:** Restricted & Standard Administrative Units (AUs)
- **Workforce Resource Mappings:** Security Groups & Microsoft 365 Collaborative Groups
- **Identity Security Framework:** Privileged Identity Management (PIM) Lifecycle States

---

<h2>💻 System & Lab Environment Baseline</h2>

- **Identity Cloud Provider:** Microsoft Entra ID Developer Sandbox Tenant
- **Enterprise Root Domain:** `syskko.onmicrosoft.com`
- **Host Endpoint Platform:** Windows 11 Enterprise (Secure Administrative Workstation)
- **Access Context:** Isolated Multi-Session Web Browsing (InPrivate/Incognito Handshakes)

---

<h2>🎯 Core Engineering Objectives</h2>

1. **Isolate Regional Directory Management:** Provision a dedicated New York Administrative Unit (`NY-Administrative-Unit`) to serve as a secure regional boundary.
2. **Implement Scoped Privileged Identities:** Assign the `Hybrid Identity Administrator` role to a regional lead, restricting administrative power strictly to the New York container.
3. **Deploy Enterprise Workforce Groups:** Create a dedicated Security Group (`NY-Staff-Group`) to manage permissions and access baselines for regional personnel in bulk.
4. **Delegate Collaborative Data Ownership:** Establish a Microsoft 365 Group (`NY-Marketing-Group`) and assign a standard non-admin employee as the owner to delegate day-to-day roster operations.
5. **Validate Perimeter Security Boundaries:** Log in as both the scoped regional admin and the standard group owner to run a validation matrix, confirming that neither account can break out of its assigned directory scope.

---

<h2>🏗️ Architecture & Privilege Segmentation Overview</h2>

<p align="center">
<b>NexusCorp Global - New York Regional IAM Structure</b> <br/>
<img src="images/step1.png" height="80%" width="80%" alt="Architecture Overview"/>
</p>

---

<h2>🚀 Step-by-Step Enterprise Implementation Walkthrough</h2>

### 🏁 Phase 0: The Prerequisites Checklist. Before running any commands, To ensure my environment is configured correctly:

- Open your Microsoft Entra Admin Center (entra.microsoft.com) using my Global Administrator account.
- Open Windows PowerShell on my local machine by right-clicking it and selecting Run as Administrator.
- Have a text editor open (like Notepad or VS Code) to prepare a CSV file.

<p align="center">
<img src="Images/step 1.jpg" alt="Install the SDK Modules
"/>
</p>






























## 🔒 Security Concepts & Governance Matrix Demonstrated

| Implemented Security Mechanism | Architectural Enforcement Method | Mitigated Corporate Threat Vector |
| :--- | :--- | :--- |
| **Least Privilege Access (PoLP)** | Stripped global roles and deployed a containerized `Hybrid Identity Administrator` role. | Prevents lateral privilege escalation across non-identity tenant environments. |
| **Scoped Directory Perimeters** | Enforced regional boundaries via the `NY-Administrative-Unit` container configuration. | Mitigates global configuration drift caused by localized regional technicians. |
| **Delegated Resource Ownership** | Assigned standard users as Group Owners over Microsoft 365 collaborative containers. | Eliminates administrative bottlenecks by offloading routine roster management to department heads. |
| **Separation of Duties (SoD)** | Isolated infrastructure security group modifications from collaborative group spaces. | Prevents unauthorized data access or malicious group-nesting permission overrides. |
| **Access Boundary Isolation** | Hardened group-level controls to prevent cross-boundary modifications by unauthorized profiles. | Blocks insider threats and unauthorized modifications to critical corporate user lists. |

---

## 📊 Summary of Validation Matrix Results

| Verified Access Constraint Condition | Testing Context Account | Applied Access Layer Path | Verified Operational Result | Status |
| :--- | :--- | :--- | :--- | :---: |
| **Intra-Boundary Security Group Modification** | `Bob Jones` (Scoped Admin) | `NY-Administrative-Unit` $\rightarrow$ `NY-Staff-Group` | Access Granted: Successfully appended user `brunocapson` to the roster. | **ACTIVE / PASS** |
| **Collaborative Group Modification Boundary Block** | `Bob Jones` (Scoped Admin) | `NY-Administrative-Unit` $\rightarrow$ `NY-Marketing-Group` | Access Denied: System grayed out membership control switches. | **SECURED / PASS** |
| **Delegated Object-Level Resource Control** | `Greg Johnson` (Standard Owner) | Global Groups View $\rightarrow$ `NY-Marketing-Group` | Access Granted: Successfully appended user `sarahlee` to the roster. | **ACTIVE / PASS** |
| **Infrastructure Protection Boundary Block** | `Greg Johnson` (Standard Owner) | Global Groups View $\rightarrow$ `NY-Staff-Group` | Access Denied: Command buttons disabled; zero external modification allowed. | **SECURED / PASS** |

---

## 📚 Core Engineering Lessons Learned

1. **Separation of Roles vs. Separation of Scope:** I discovered that granting an administrative role at the tenant level poses an unnecessary security risk. By combining specialized roles with **Administrative Unit Scopes**, organizations can delegate targeted technical capabilities to regional teams without exposing global configurations.
2. **The Power of Resource-Level Delegation:** I demonstrated that delegating explicit object ownership allows business units to manage their daily collaboration spaces independently. This reduces routine ticket queues for central IT helpdesks while maintaining strict security boundaries.
3. **Implicit vs. Explicit Administrative Power:** I validated that when a group is nested inside an Administrative Unit, scoped administrators inherit full management authority over it automatically. This makes assigning an explicit group owner redundant, allowing for a cleaner and more auditable security design.

---

## 🚧 Future Architectural Enhancements

To scale this regional governance model into an enterprise-grade, fully automated framework, the next iterations of this project will integrate:
- **Programmatic Lifecycle Automation:** Transitioning GUI staging tasks into automated, declarative **Microsoft Graph PowerShell SDK** onboarding scripts.
- **Dynamic Governance Rules:** Implementing advanced OData syntax strings (`device.deviceOSType -contains "Windows"`) to automate secure hardware grouping.
- **Automated Lifecycle Ingestion:** Developing automated flat-file parsing loops to synchronize directory states directly with HR data sources.

---

## 🤝 Connect With Me

Let's discuss identity security, cloud governance architecture, and zero-trust engineering implementations:

<p align="left">
<a href="https://linkedin.com/in/divine-oguamanam-a21765337" target="blank">
<img align="center" src="https://cdn.jsdelivr.net/npm/simple-icons@v3/icons/linkedin.svg" height="30" width="40" alt="LinkedIn Profile" />
</a>

<a href="https://twitter.com/syskko" target="blank">
<img align="center" src="https://cdn.jsdelivr.net/npm/simple-icons@v3/icons/twitter.svg" height="30" width="40" alt="Twitter Profile" />
</a>
</p>
