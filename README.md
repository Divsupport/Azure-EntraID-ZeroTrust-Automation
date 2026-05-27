
md = r'''<h1 align="center">🔐 Enterprise Identity Lifecycle Automation (Joiner-Mover-Leaver Pipeline)</h1>

<h3 align="center">
Built by <span style="color:#0078D4;">Divine Oguamanam</span>
</h3>

---

## Project Overview

This project shows how to build a full identity lifecycle system in **Microsoft Entra ID** using **Microsoft Forms** and **Power Automate**.

The goal is simple.  
Stop doing identity work by hand.

In many companies, HR sends emails to IT when someone joins, changes roles, or leaves.  
That process is slow. It creates mistakes. It also leaves security gaps.

This project replaces that manual work with one automated pipeline for:

- **Joiner**, when a new employee joins
- **Mover**, when an employee changes role or department
- **Leaver**, when an employee leaves the company

The automation follows the identity lifecycle from start to end.  
It creates accounts, updates user data, moves people between groups, and locks accounts when needed.

### Business Problem

Before automation, identity work was handled manually.

That caused:

- Slow onboarding
- Wrong group access
- Incorrect license assignments
- Forgotten offboarding
- Security risk from old accounts still active
- Extra work for IT and helpdesk teams

### Solution

I built a zero-touch identity flow with:

- **Microsoft Forms** as the data entry point
- **Power Automate** as the workflow engine
- **Microsoft Entra ID** as the identity system
- **Dynamic and static group actions** for access control
- **Attribute-based decisions** to keep access clean and organized

### What this project proves

- New users can be created from a form
- Existing users can be updated when their role changes
- Group membership can be changed automatically
- Leaving users can be disabled and removed from access groups
- Identity work can be handled with less manual effort and fewer mistakes

### Key Security Ideas

- **Least Privilege**: Give people only the access they need
- **Separation of Duties**: Keep HR, IT, and business ownership separate
- **Automation**: Reduce manual steps and human error
- **Lifecycle Control**: Handle identity changes at the right time
- **Access Clean-Up**: Remove old access when users move or leave

---

## Technologies, Frameworks, and Tools

- **Identity Platform:** Microsoft Entra ID
- **Automation Platform:** Microsoft Power Automate
- **Intake Tool:** Microsoft Forms
- **Admin Console:** Microsoft Entra Admin Center
- **Access Model:** Group-based access control
- **Workflow Style:** Joiner, Mover, Leaver lifecycle automation
- **Host Environment:** Windows desktop with browser-based admin tools

---

## System and Lab Environment Baseline

- **Tenant:** Microsoft Entra ID developer tenant
- **Domain:** `syskko.onmicrosoft.com`
- **Admin Access:** Global Administrator account for setup
- **Client Device:** Windows 11 workstation
- **Browser Mode:** Regular and private browser sessions for testing
- **Testing Style:** Manual validation after each workflow phase

---

## Core Engineering Objectives

1. **Automate onboarding** for new hires with a form-driven flow.
2. **Automate role changes** when an employee moves to a new team or department.
3. **Automate offboarding** when an employee leaves the company.
4. **Keep identity data clean** by mapping form answers to directory fields.
5. **Reduce security risk** by removing old access as soon as a lifecycle event happens.
6. **Prove every step** with testing and directory validation.

---

## Identity Lifecycle Architecture

```text
[HR Form Submission]
        ↓
[Power Automate Flow]
        ↓
[Microsoft Entra ID]
        ↓
[Group Membership Updates]
        ↓
[Access Granted, Changed, or Removed]








































<img src="images/step 4.jpg"/> <br />



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
