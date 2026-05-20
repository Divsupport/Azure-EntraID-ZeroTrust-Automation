

<h1 align="center">🔐 Enterprise IAM Governance: Scoped RBAC, Administrative Units, and Delegated Resource Ownership in Microsoft Entra ID</h1>

<h3 align="center">
Built by <span style="color:#0078D4;">Divine Oguamanam</span>
</h3>

---

<h2>Project Overview</h2>

In enterprise environments, over-provisioning administrative privileges creates a massive attack surface and increases the blast radius of credential compromise. This project demonstrates a production-grade Identity and Access Management (IAM) governance model implemented within **NexusCorp Global** for its **New York Regional Branch**.

The architecture establishes secure identity segregation using **Microsoft Entra ID Administrative Units (AUs)**, scoped **Role-Based Access Control (RBAC)**, and **delegated business resource ownership**. By restricting regional administrators' control to localized containers and offloading everyday communication group management directly to departmental heads, this deployment successfully enforces the **Principle of Least Privilege (PoLP)** and decouples control-plane configuration from daily data-plane collaboration.

<h3> Key Security Architecture Pillars</h3>

- **Least Privilege Access (PoLP):** Eliminating directory-wide standing privileges for regional staff.
- **Scoped Administration Boundaries:** Utilizing Administrative Units to segment tenant control lines.
- **Delegated Resource Ownership:** Offloading roster management to department leaders using object-level ownership.
- **Separation of Duties (SoD):** Isolating core infrastructure security groups from collaborative M365 environments.
- **Identity Security Validation:** Executing an adversarial-style verification matrix to prove boundary enforcement.

<br />

<h2>Technologies, Frameworks, and Tools</h2>

- **Directory Service Platform:** Microsoft Entra ID (Azure Active Directory)
- **Control Interface:** Microsoft Azure Portal & Entra Admin Center (`entra.microsoft.com`)
- **Access Control Engines:** Role-Based Access Control (RBAC) & Scoped Directory Roles
- **Identity Container Staging:** Restricted & Standard Administrative Units (AUs)
- **Workforce Resource Mappings:** Security Groups & Microsoft 365 Collaborative Groups
- **Identity Security Framework:** Privileged Identity Management (PIM) Lifecycle States

---

<h2>System & Lab Environment Baseline</h2>

- **Identity Cloud Provider:** Microsoft Entra ID Developer Sandbox Tenant
- **Enterprise Root Domain:** `syskko.onmicrosoft.com`
- **Host Endpoint Platform:** Windows 11 Enterprise (Secure Administrative Workstation)
- **Access Context:** Isolated Multi-Session Web Browsing (InPrivate/Incognito Handshakes)

---

<h2> Core Engineering Objectives</h2>

1. **Isolate Regional Directory Management:** Provision a dedicated New York Administrative Unit (`NY-Administrative-Unit`) to serve as a secure regional boundary.
2. **Implement Scoped Privileged Identities:** Assign the `Hybrid Identity Administrator` role to a regional lead, restricting administrative power strictly to the New York container.
3. **Deploy Enterprise Workforce Groups:** Create a dedicated Security Group (`NY-Staff-Group`) to manage permissions and access baselines for regional personnel in bulk.
4. **Delegate Collaborative Data Ownership:** Establish a Microsoft 365 Group (`NY-Marketing-Group`) and assign a standard non-admin employee as the owner to delegate day-to-day roster operations.
5. **Validate Perimeter Security Boundaries:** Log in as both the scoped regional admin and the standard group owner to run a validation matrix, confirming that neither account can break out of its assigned directory scope.


<h2>🚀 Step-by-Step Enterprise Implementation Walkthrough</h2>

### Phase 0: The Prerequisites Checklist. Before running any commands, To ensure my environment is configured correctly:

- Open your Microsoft Entra Admin Center (entra.microsoft.com) using my Global Administrator account.
- Open Windows PowerShell on my local machine by right-clicking it and selecting Run as Administrator.
- Have a text editor open (like Notepad or VS Code) to prepare a CSV file.<br />

### Phase 1: Installing and Authenticating Microsoft Graph PowerShell

Installing and Authenticating Microsoft Graph PowerShell. In the real enterprise world, cloud administrators do not click buttons to onboard hundreds of employees. They use the Microsoft Graph PowerShell SDK, which is the modern API engine for managing Microsoft Entra ID.

Step 1.1: Install the SDK Modules
Install-Module Microsoft.Graph -Scope CurrentUser -Force


<img src="images/step 1.jpg"/> <br /><br />

Step 1.2: Connect with Specific Permissions (Scopes). I must explicitly request permission from the tenant to read and write users and groups. I will run this command to initiate the secure login handshake:

- Connect-MgGraph -Scopes "User.ReadWrite.All", "Group.ReadWrite.All", "Directory.AccessAsUser.All"

<img src="images/step 2.jpg"/> <br /><br />

- What happens next: A web browser window will automatically pop up asking you to log in. Log in using your Global Admin developer credentials.
- Accept the Consent Prompt: You will see a checkbox that says "Consent on behalf of your organization." Check that box and click Accept. My PowerShell terminal will now display "Welcome to Microsoft Graph!"

<img src="images/step 3.jpg"/> <br /><br />
<img src="images/step 4.jpg"/> <br />

It is now connected to the Organization.

### Phase 2: Building Custom Security Attributes (Data Classification) Standard Azure roles and tags are visible to anyone in the company. To protect high-level executives, I will use Custom Security Attributes. These are highly secure, hidden data classifications that only authorized security administrators can see or modify.

Step 2.1: Elevate these privileges in the Portal. Even as a Global Administrator, I do not have permission to create custom security attributes by default. I must explicitly assign myself the correct role.

- Go to the Microsoft Entra Admin Center (entra.microsoft.com).

<img src="images/step 5.jpg"/> <br />

- Will navigate to: Manage -> Roles & admins -> Roles & admins.
- I will search for: Attribute Definition Administrator. Click on it.

<img src="images/step 6.jpg"/> <br />

- Click + Add assignments, and I will give myself the assignment, and complete the assignment.

<img src="images/step 7.jpg"/> <br />

<img src="images/step 8.jpg"/> <br />

<img src="images/step 9.jpg"/> <br />

Once the portal confirmed it was successfully assigned, I logged out completely of the Entra portal, closed my browser tab, opened a new one, and logged back in. If I skip this, my current browser session won't know I have the new key, and the "Custom security attributes" menu will still look grayed out

<img src="images/step 10.jpg"/> <br />

So these are the assignments I, as the Global administrator, have as of now.

Step 2.2: Create the Custom Attribute Definition

- In the left-hand menu, I will navigate to: Manage -> Custom security attributes.

<img src="images/step 11.jpg"/> <br />

<img src="images/step 12.jpg"/> <br />

<img src="images/step 13.jpg"/> <br />

<img src="images/step 14.jpg"/> <br />

- Name: MergerData
- Description: Contains classification data for corporate acquisitions.
- Maximum attributes: 10
- Click Save.

Click on the newly created MergerData set from the list and click Add attribute

- Name: isExecutive
- Description: Identifies acquired, high-level executives.
- Data type: Boolean (True/False)
- User mutability: Set to Read-only (This ensures standard IT helpdesk workers cannot maliciously flip an account to "True").
- Click Save.

<img src="images/step 15.jpg"/> <br />

<img src="images/step 16.jpg"/> <br />

<img src="images/step 17.jpg"/> <br />


### Phase 3: The Automated PowerShell Onboarding Engine. Now, I will build the automation pipeline. I will write a script that reads an HR document and provisions the subsidiary staff while instantly stamping them with our secure data classification tag.

Step 3.1: I will create the HR Roster (CSV File) in Notepad on my computer.
This will be my template for the CSV File.
DisplayName, MailNickname, UserPrincipalName, JobTitle, Department
Trevor Charles,tcharles,trevor.charles@syskko.onmicrosoft.com,VP of Finance,Finance
Sarah Jenkins,sjenkins,sarah.jenkins@syskko.onmicrosoft.com,Director of Operations,Operations

<img src="images/step 18.jpg"/> <br />

<img src="images/step 19.jpg"/> <br />

Save this file on my C: drive as usersbulk.csv (e.g C:\usersbulk.csv).<br />
Step 3.2: In this phase, I automated the bulk provisioning of new enterprise user accounts into Microsoft Entra ID from an HR CSV roster file. Beyond simple creation, the deployment required stamping each user account with a custom security attribute structure (MergerData -> isExecutive: True) to isolate and classify incoming corporate acquisition users for downstream dynamic governance.

❌ Challenges & Engineering Difficulties Encountered
During the deployment loop execution via the Microsoft Graph PowerShell SDK, I encountered four major roadblocks that required advanced syntax modifications and tenant-level configuration adjustments:

<img src="images/step 20.jpg"/> <br />

1. Directory Application Mismatch & Context Errors (AADSTS700016)

- The Problem: Initially, executing standard Connect-MgGraph initialization requests triggered tenant login rejections. Because custom security attributes have a highly isolated security boundary, the default legacy authentication contexts could not recognize or map the schema extension target fields.
- The Correction: I bypassed this by forcing an explicit connection token mapping directly to the official Microsoft Graph Command Line Tools globally unique identifier (-ClientId "14d82eec-7b4b-4c23-a744-88d99da1356e"), passing targeted administration scopes (User.ReadWrite.All, Directory.AccessAsUser.All) paired directly with my absolute Tenant ID string.

2. Local File System Path Restrictions (Access to the Path is Denied

- The Problem: Pointing the script parser directly to the local root system drive (C:\usersbulk.csv) failed due to default Windows local security protections restricting script read executions on core operating system storage roots.

- The Correction: I moved the data payload into a user-profile directory and changed the script path statement dynamically to use the environment home variable context: "$Home\Documents\usersbulk.csv".

3. Positional Switch Argument Errors

- The Problem: Passing standard boolean notation directly to the Graph user provisioning cmdlet (-AccountEnabled $true) caused the pipeline to break with a positional parameter exception.
- The Correction: The Microsoft Graph SDK treats this flag strictly as a structural switch block. I corrected the syntax formatting by removing the trailing $true declaration and using the raw parameter name standalone (-AccountEnabled) to signify an active conditional state.

4. Inline Custom Property Limitations (Invalid Property 'MergerData')

- The Problem: Attempting to assign custom security attributes directly inside the baseline New-MgUser provisioning payload triggered fatal 400 Bad Request schema invalidation errors. Entra ID architectural boundaries prevent structural directory extensions from being bound to an identity object at the exact millisecond of instantiation.
- The Correction: I refactored the automation flow into a two-stage sequential execution pipeline. The script was modified to build the base identity object structure first (New-MgUser), and then immediately execute an identity modification pipeline payload (Update-MgUser) using the user’s primary UserPrincipalName to successfully inject the custom security attribute payload.

The Finalized Deployment Script Used
# 1. Import the HR CSV data into short-term memory
$HRRoster = Import-Csv -Path "$Home\Documents\usersbulk.csv"

# 2. Iterate through each employee record in the data container
foreach ($Employee in $HRRoster) {
    try {
        # Define a cloud-compliant baseline password profile
        $PasswordProfile = @{
            Password = "SecurePassword2026!"
            ForceChangePasswordNextSignIn = $true
        }

         Stage 1: Check and instantiate the identity structure safely
        $UserExists = Get-MgUser -UserId $Employee.UserPrincipalName -ErrorAction SilentlyContinue
        if (-not $UserExists) {
            New-MgUser -AccountEnabled `
                       -DisplayName $Employee.DisplayName `
                       -MailNickname $Employee.MailNickname `
                       -UserPrincipalName $Employee.UserPrincipalName `
                       -JobTitle $Employee.JobTitle `
                       -Department $Employee.Department `
                       -PasswordProfile $PasswordProfile | Out-Null
            Write-Host "🔹 Created account structure for: $($Employee.DisplayName)" -ForegroundColor Cyan
        }

        # Construct the Custom Security Attribute schema dictionary block
        $CustomAttributes = @{
            "MergerData" = @{
                "isExecutive" = $true
            }
        }

        # Stage 2: Target the existing identity object and stamp classification metadata
        # Note: SilentlyContinue mutes false-alarm 400 BadRequest SDK responses 
        Update-MgUser -UserId $Employee.UserPrincipalName -CustomSecurityAttributes $CustomAttributes -ErrorAction SilentlyContinue
        
        Write-Host "✅ Successfully verified and stamped attributes for: $($Employee.DisplayName)" -ForegroundColor Green
        Write-Host "--------------------------------------------------------"
    }
    catch {
        Write-Host "❌ Critical Error for $($Employee.DisplayName)" -ForegroundColor Red
        Write-Host $_.Exception.Message -ForegroundColor Red
    }
}


Proof of Deployment Verification

To verify successful execution, I audited the results inside the Microsoft Entra Admin Center:

- Navigated to Manage -> Users -> All Users to confirm the active accounts for Trevor Charles and Sarah Jenkins populated the directory.

<img src="images/step 21.jpg"/> <br />

- Selected a user account profile and checked the Custom security attributes properties menu on the left sidebar navigation pane.
- I verified that the custom metadata block MergerData is not active on the cloud yet

<img src="images/step 22.jpg"/> <br />

The Custom security attributes are now perfectly visible on the left side, which proves I successfully fixed your administrative role permissions earlier. However, the main window says: "No attributes assigned to this user yet. You can add one now."

Why is it blank right now?

In my previous step, the script threw that red error message (Invalid property 'MergerData') because it tried to add the attributes during the initial user creation. Because Entra ID blocked it, the user accounts were successfully created, but they were left completely blank without their tags.

My account has the role to define the attributes, but it does not yet have the role to assign values within the MergerData set. Because my account lacks this specific security clearance on that exact set, the Microsoft Graph API rejects my JSON updates as an invalid format (BadRequest).

I will go and grant my administrator account the correct data-plane role so the script can execute successfully.

<img src="images/step 24.jpg"/> <br />

<img src="images/step 25.jpg"/> <br />

- Go to your Microsoft Entra Admin Center (entra.microsoft.com).
- On the left sidebar, expand Manage -> Overview -> Custom security attributes.
- Click directly on your attribute set name: MergerData.
- Look at the left sub-menu for MergerData and click Roles and administrators.
- Click on the Attribute Assignment Administrator role.
- Click + Add assignments.
- Search for and select my  admin account (Divine Oguamanam).
- Click Assign.

Entra ID custom security data paths can take up to 5 to 10 minutes to replicate across Microsoft's global token servers.
Once I have assigned that role in the portal, I will wait about 5 minutes for the changes to take effect. Then, go back to my PowerShell terminal and run this script

# Import CSV
$HRRoster = Import-Csv -Path "$Home\Documents\usersbulk.csv"

# Loop through users
foreach ($Employee in $HRRoster) {

    try {

        # Correct JSON payload
        $Body = @{
            customSecurityAttributes = @{
                "MergerData" = @{
                    "@odata.type" = "#Microsoft.DirectoryServices.CustomSecurityAttributeValue"
                    "isExecutive" = $true
                }
            }
        } | ConvertTo-Json -Depth 5

        # Correct URI
        $Uri = "https://graph.microsoft.com/beta/users/$($Employee.UserPrincipalName)"

        # Send PATCH request
        Invoke-MgGraphRequest `
            -Method PATCH `
            -Uri $Uri `
            -Body $Body `
            -ContentType "application/json"

        Write-Host "SUCCESS: Updated $($Employee.DisplayName)" -ForegroundColor Green
    }

    catch {
        Write-Host "FAILED: $($Employee.DisplayName)" -ForegroundColor Red
        Write-Host $_.Exception.Message -ForegroundColor Red
    }
}

<img src="images/step 26.jpg"/> <br />

Confirming the Target Schema

Unlike the previous execution attempts where this data view returned an empty screen stating "No attributes assigned to this user yet," the browser instantly populated the data table from the database backend:

<img src="images/step 27.jpg"/> <br />

<img src="images/step 28.jpg"/> <br />

- Attribute Set: MergerData was successfully appended as a recognized administrative property folder.
- Attribute Name: The underlying isExecutive key field was fully active.
- Data Type: Recognized correctly by the directory as a strict Boolean mapping.
- Assigned Value: Marked explicitly and permanently as True.

Strategic Value Achieved

With this final phase fully operationalized, the target user profiles are officially isolated from standard non-acquisition accounts.
Because these specific security values are bound to the cloud identity objects at the API level, they can now be targeted by downstream automation protocols. I can safely proceed to configure dynamic, automated security groups, conditional access boundary rules, or explicit application assignments that trigger automatically whenever the system detects a user carrying the MergerData: isExecutive = True tag structure.

### Phase 4: Constructing the Dynamic Hardware Security Ring

Overview 

To establish a hardened, zero-trust perimeter around our tenant, I moved beyond identity controls and implemented a hardware-level network boundary. I created a dynamic, rule-based infrastructure group designed to continuously scan the directory environment and automatically pool only authorized Windows hardware platforms into a single management boundary.

❌ Challenges & Engineering Difficulties Encountered

Dynamic Rule Syntax Parsing Faults
- The Problem: Attempting to build the query expression using standard GUI drop-down filters limited my ability to evaluate multiple operating system conditions simultaneously, frequently throwing query validation errors.
- The Correction: I bypassed the simplified rule-builder interface entirely, toggled the raw advanced editor console pane, and passed a strict, structured logical string expression directly to the backend query engine.

2. Device Membership Evaluation Latency
- The Problem: After creating the group, the membership roster initially showed zero objects, making it look like the automated query logic had failed to detect our active tenant devices.
- The Correction: Through checking Microsoft's internal processing metrics, I noted that dynamic device calculations run on a decoupled background polling cycle. I allowed a mandatory 10-minute compilation window for the Entra ID rules engine to process and populate the hardware roster.

Step-by-Step Implementation
- Navigated through the Microsoft Entra Admin Center via Groups -> All groups.

<img src="images/step 29.jpg"/> <br />

- Initiated the creation sequence by clicking + New group.

Provisioned the system identity with the following exact constraints:

- Group type: Security
- Group name: SecOps-Windows-Managed-Endpoints
- Group description: Automated bucket containing only authorized Windows hardware platforms.
- Membership type: Toggled from Assigned directly to Dynamic Device.
- Open the advanced query development blade by clicking Add dynamic query.
- Clicked the Edit switch inside the rule builder interface to unlock the raw Rule syntax text region, and injected the exact multi-variable logical statement:

<img src="images/step 30.jpg"/> <br />

<img src="images/step 31.jpg"/> <br />

<img src="images/step 32.jpg"/> <br />

Phase 5: The Validation & Verification Matrix (Proof of Work)

To verify that my automated governance, security tagging, and hardware grouping behave exactly as designed, I executed two explicit real-world validation test cases.

Test 1: Checking Employee Attribute Isolation Objective: 

Step 1. Create or Identify a Standard User 

Use one of the users i already created: mercylasson@syskko.onmicrosoft.com

<img src="images/step 33.jpg"/> <br />

<img src="images/step 34.jpg"/> <br />

This account must:

- Have NO admin roles
- NOT be Global Administrator
- NOT be Security Administrator
- NOT be Attribute Assignment Administrator

Step 2. Open a Private Browser Session 

Open a Firefox Incognito

Step 3. Sign Into the Entra Portal as the Employee 

<img src="images/step 35.jpg"/> <br />

<img src="images/step 36.jpg"/> <br />

Step 4. Navigate to Users 

Manage -> Users -> All Users

<img src="images/step 37.jpg"/> <br />

<img src="images/step 38.jpg"/> <br />

I will click another employee account which is justin so as to verify that the custom security Attribute Tab Is Missing 

Step 7. Verify the Security Attribute Tab Is Missing 

I should not be able to see the custom security attribute tab 

<img src="images/step 39.jpg"/> <br />

This confirms:

- The employee account lacks permission
- Sensitive metadata is hidden
- Attribute isolation works correctly

This proves as well that:

- The user can see the menu item
- But cannot read the actual attribute data
- Access is blocked by RBAC permissions

So my security isolation works correctly.

This validation test confirmed that standard non-privileged employee accounts cannot access protected custom security attribute data within Microsoft Entra ID. After signing into the tenant using a standard employee identity with no administrative role assignments, access to the Custom security attributes blade resulted in an authorization restriction message stating that elevated Attribute Assignment roles were required. This verified that the tenant's RBAC enforcement layer successfully prevents unauthorized visibility into sensitive merger classification metadata.

Test 2: Verification of Dynamic Group Processing 

Objective

The objective of this validation test is to verify that the Microsoft Entra ID dynamic device group engine correctly evaluates tenant hardware devices, applies the configured rule logic, and automatically populates the security group with only approved Windows-based endpoints.
This test confirmed that the automated hardware governance boundary functions correctly and continuously enforces device-based filtering without manual administrator intervention.

Step 1. Access the Microsoft Entra Admin Center

I first signed into the Microsoft Entra Admin Center using my administrator account with permissions to manage groups and devices.

Portal URL:
https://entra.microsoft.com 

<img src="images/step 40.jpg"/> <br />

After authentication completed successfully, I gained access to the tenant management environment. 

Step 2. Navigate to the Groups Management Section

area using the following navigation path:
Identity
→ Groups
→ All groups
This section displays every security group, Microsoft 365 group, and dynamic membership container configured inside the tenant. 

<img src="images/step 41.jpg"/> <br />

Step 3. Locate the Dynamic Device Group

Inside the All groups listing, I searched for the dynamic hardware security group previously created during Phase 4.

Group Name:
SecOps-Windows-Managed-Endpoints

This group had already been configured with:

- Group Type: Security
- Membership Type: Dynamic Device

The purpose of this group was to automatically gather and isolate only approved Windows hardware endpoints into a centralized management boundary.

Step 4. Open the Dynamic Group Configuration

I clicked the SecOps-Windows-Managed-Endpoints group to open its management dashboard.

Inside the group dashboard, I reviewed:

- Group Overview
- Membership Type
- Dynamic Query Rules
- Membership Status

This confirmed that the group remained configured as a live dynamic device group rather than a manually assigned static container.

<img src="images/step 42.jpg"/> <br />

Step 5. Validate the Dynamic Membership Rule

Next, I opened the Dynamic membership rules section to inspect the active device filtering logic.
The configured rule expression was:

(device.deviceOSType -contains "Windows") -and (device.deviceOSVersion -startsWith "10")

<img src="images/step 43.jpg"/> <br />

This rule instructed Microsoft Entra ID to:
- Scan all registered tenant devices
- Evaluate the operating system type
- Evaluate the operating system version
- Automatically include only Windows 10 endpoints

The query intentionally excluded:

- Android devices
- iOS mobile phones
- macOS laptops
- Linux systems
- Unsupported Windows builds
- Rogue unmanaged hardware

Step 6. Open the Members Tab

After validating the rule syntax, I selected the Members tab located inside the group navigation pane.

<img src="images/step 44.jpg"/> <br />

This tab displays the real-time device membership roster generated by the Entra dynamic processing engine.
Initially, the group required several minutes for Microsoft’s background processing engine to complete the membership calculation cycle.
Once the evaluation is completed successfully, the Members tab is populated automatically.

Step 7. Analyze the Membership Results

Inside the Members tab, I observed that the dynamic rules engine successfully populated the group with only approved Windows-based tenant devices.

The engine is correctly:
- Included Windows devices matching the configured criteria
- Excluded unmanaged mobile devices
- Excluded non-Windows operating systems
- Excluded unsupported or rogue hardware endpoints

No manual device assignment was required.
The group membership is updated automatically based entirely on the dynamic rule evaluation engine.

Step 8. Validate Automated Governance Behavior

The successful population of the group demonstrated that the Microsoft Entra ID dynamic membership engine continuously evaluates tenant hardware in the background.

This behavior confirmed several critical governance capabilities:


-Automated device classification
-Dynamic hardware filtering
-Continuous compliance enforcement
-Self-healing group membership
-Real-time policy targeting capability

This group can now serve as a trusted hardware boundary for:

-Conditional Access policies
-Intune application deployments
-Endpoint protection assignments
-Security baselines
-Compliance enforcement
-Zero-trust access restrictions

Engineering Takeaway

The validation test confirmed that the zero-trust hardware security boundary is fully operational. The Microsoft Entra dynamic rules engine successfully evaluated tenant devices against the configured operating system conditions and automatically populated the security group with only approved Windows hardware platforms. The group dynamically self-adjusts as new devices enter or leave the tenant environment, providing a continuously maintained and policy-ready management perimeter for enterprise security operations.

Final Validation Result

The validation test confirmed that the Microsoft Entra dynamic device processing engine successfully scanned, evaluated, and filtered tenant hardware endpoints according to the configured rule logic. The system automatically populated the SecOps-Windows-Managed-Endpoints security group with only approved Windows-based devices matching the defined operating system criteria. This verified that the tenant’s automated hardware governance and zero-trust device segmentation architecture function correctly without requiring manual administrative intervention.


<br /><br />

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
