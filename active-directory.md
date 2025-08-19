<h1 = align=center>𝙷𝙾𝙼𝙴 𝙻𝙰𝙱 - 𝙰𝙲𝚃𝙸𝚅𝙴 𝙳𝙸𝚁𝙴𝙲𝚃𝙾𝚁𝚈</h1>
<h2 = align=center>Bulk User Provisioning with PowerShell</h2>

<p align="center">
<img width="1342" height="883" alt="Untitled Diagram drawio (3)" src="https://github.com/user-attachments/assets/978f6667-84a3-403e-b02b-9328f6341827" />
</p>
---

## 🛠️ TECHNOLOGY & TOOLS UTILIZED

- **`VirtualBox:`**  
  Used as the primary virtualization platform to host multiple virtual machines in an isolated local environment for cybersecurity testing and infrastructure simulation.

- **`Windows Server 2019:`**  
  Deployed as the domain controller within the lab. Configured to run Active Directory Domain Services (AD DS), DNS, and Group Policy to emulate enterprise-level identity and access management scenarios.

- **`Windows 10:`**  
  Installed as a domain-joined workstation to simulate a typical enterprise endpoint. Used for user behavior simulation, GPO testing, and endpoint visibility through logging tools.

- **`Active Directory:`**  
  Implemented to manage user accounts, groups, and organizational units (OUs). Used for hands-on experience with authentication flows, access control, privilege escalation testing, and administrative scripting.

- **`PowerShell:`**  
  Used for automating administrative tasks such as creating users, managing AD objects, and configuring system settings within both the server and client machines.
  
- **`Group Policy Management:`**  
  Applied GPOs to enforce password policies, audit policies, restrict user privileges, and configure security baselines across the domain environment.

---

## OBJECTIVE

Developed and deployed a virtualized Windows domain lab using a hypervisor - `VirtualBox`, `Windows Server 2019` for the DHCP, and `Windows 10` for the client machine to gain hands-on experience with enterprise network infrastructure and  identity management. The project focused on `Active Directory`, `Group Policy`, and domain-joined `endpoints` to simulate real-world authentication, access control, and administrative workflow. This lab environment essentially created a account and password provisioning setup with common security practices such  as priviledge escalation.

## SETTING UP THE DOMAIN CONTROLLER
### STEP 1: `Windows Server 2019` Virtual Machine Creation and Provisioning

Virtual Machine - named as `Domain Controller`, was configured as a domain controller using the `Windows Server 2019 ISO`. The information of the provisioning is 2040 MB of RAM and a virtual CPU to support the `Active Directory`  and core infrastructure services.

