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

---
## SETTING UP THE DOMAIN CONTROLLER
### STEP 1: `Windows Server 2019` Virtual Machine Creation and Provisioning

Virtual Machine - named as `Domain Controller`, was configured as a domain controller using the `Windows Server 2019 ISO`. The information of the provisioning is 2040 MB of RAM and a virtual CPU to support the `Active Directory`  and core infrastructure services.

---
## Step 2: Configure `Network Adapters`
For network setup, `Network Adapters 1`- enabled and attached `NAT` to allow internet access and `Network Adapter 2` -  connect to an `internal network` 

<img width="1131" height="471" alt="Lab 4" src="https://github.com/user-attachments/assets/c379a872-b1dc-4bb6-880b-a0917fe99f13" /></br>
<img width="1131" height="471" alt="Lab 4" src="https://raw.githubusercontent.com/DanielTsang26/home-lab/refs/heads/main/Screenshot%202025-08-19%20112215.png" /></br>

---

## Step 3: `Windows Setup`

For setup, this was essentially powered by the virtual machine and connected by the Windows Server 2019 installation  process using the ISO image.

<img width="636" height="476" alt="Lab 6" src="https://github.com/user-attachments/assets/31f0efb0-f514-40d9-8987-d7ba2b00bff5" /></br>

*Windows Server 2019 Standard Evaluation (Desktop Experience) -> Accept the license term -> Username: administrator Password: dT!181290*

---

## Step 4: `Network Configuration` (IP Subnet Masking)

Checked the `network conection details` of the virtual machine and observed it was automatically assigned to IPv4 address.
Under the Network connection, we renamed the NAT adapter to `_INTERNAL_` for clarity. The `_INTERNAL_` adapter was then manually configuered with the following settings:

- **IP address:** `172.16.0.1`  
- **Subnet mask:** `255.255.255.0`  
- **Preferred DNS server:** `127.0.0.1`

<img width="782" height="159" alt="Lab 10" src="https://raw.githubusercontent.com/DanielTsang26/home-lab/refs/heads/main/Screenshot%202025-08-19%20122547.png" /></br>
<img width="755" height="450" alt="Lab 10" src="https://raw.githubusercontent.com/DanielTsang26/home-lab/refs/heads/main/Screenshot%202025-08-19%20123032.png" /></br>

## Step 5: Set Up `Active Directory`

In `Server Manager`, launch the `Add Roles and Features` Wizard to install `Active Directory Domain Service` (AD DS). During the setup, we also included `Group Policy Management` and `Remote Server Administrator Tools` (RSAT)  to enable centralized domain control and administrative functionality

<img width="1001" height="741" alt="Lab 12" src="https://github.com/user-attachments/assets/c5771a04-7c98-4775-b2da-d8c923f4b627" /></br>

<img width="783" height="556" alt="Lab 22" src="https://github.com/user-attachments/assets/009e3290-608c-4122-8e58-9fac3755b499" /></br>

- After role installtion, `post-deployment configuration` was implemented by promoting the server to a domain controller. During configuration, we created a new forest with the root domain name `mydomain.com`, and enabled both `Domain Name System` (DNS) server and `Global Catalog` (GC) option. we also set the NetBIOS domain name to `MYDOMAIN`

<img width="1227" height="764" alt="Lab 31" src="https://github.com/user-attachments/assets/ba1b4759-d514-4bc1-9d9b-f6541081624d" /></br>

## Step 6: Set Up `Domain Admins` and `Domain Users`

After logging back into the sesrver, we navigated  to `Active Directory Users and Computer` to begin configuring domain objects. We created a new `Organization Unit` (OU) named `_ADMIN`, and within that OU, added a new user account with the full name 'Daniel Tsang' and user log on name is `a-dtsang

<img width="1227" height="764" alt="Lab 31" src="https://raw.githubusercontent.com/DanielTsang26/home-lab/refs/heads/main/Screenshot%202025-08-19%20140706.png" /></br>

## Step 7: Set up `Remote Access` Services 

In `Service Manager`, we returned to `Add Roles and Features`  to install the `Remote Access role`. During the setup, we selected the role services ` DirectAccess and VPN` (RAS) as well as `Routing` to enable secure remote to enable secure remote connectivity and traffic management for the networks.

After that, we navigate to `Tools` -> `Routing and Remote Access` and launching the Routing and Remote Access Server Setup wizard. We proceeded to configure `NAT` by selecting our internet-facing network interface, named `Internet` , to provide network address translation for internal clients. Upon successful configuration, the  domain controller (local server) displayed a green status arrow, indicating service was running poorly.

<img width="616" height="437" alt="Lab 58" src="https://github.com/user-attachments/assets/2743e64b-b68c-41e4-b333-be1a7b70fd47" /></br>

---

## Step 8: Set up `DHCP Server`

In `Server Manager` and in `Add roles and features` Wizard again to install the `DHCP Server` role, enabling the server to assign IP addresses dynamically within the network.


We navigated to `Tools` > `DHCP` and launched the `New Scope Wizard` to configure a DHCP scope. We named the scope `172.16.0.100-200` and set the IP address range from `172.16.0.100` to `172.16.0.200` with a subnet mask of `255.255.255.0` (prefix length 24). The router (default gateway) was set to `172.16.0.1`, and the parent domain was specified as `mydomain.com`. After activating, authorizing, and refreshing the DHCP server, its status displayed a green icon indicating it was functioning properly.

<img width="167" height="172" alt="Lab 77" src="https://github.com/user-attachments/assets/cbfe1307-c73c-4d5b-9284-b3705fd0899e" /></br>

<img width="271" height="378" alt="Lab 78" src="https://github.com/user-attachments/assets/f484b260-c072-473e-9c35-a2888a5f4902" /></br>

## Step 9: Bulk User Creation with `Powershell ` in `Active Directory` 

To efficiently add 1,000+ user accounts to `Active Directory`, we used a `PowerShell` script in combination with a text file containing first and last names. The script created a new organizational unit called `_USERS` and then processed each name to generate a standardized username consisting of the user’s first initial and last name in lowercase. Each user was created with a default password `Password18` and placed within the _USERS OU. This automated method significantly streamlined bulk user provisioning, reducing manual effort and ensuring consistent account configuration across the domain.

**Script:**
```powershell
$PASSWORD_FOR_USERS   = "Password18" 
$USER_FIRST_LAST_LIST = Get-Content .\names.txt
# ------------------------------------------------------ #

$password = ConvertTo-SecureString $PASSWORD_FOR_USERS -AsPlainText -Force
New-ADOrganizationalUnit -Name _USERS -ProtectedFromAccidentalDeletion $false

foreach ($n in $USER_FIRST_LAST_LIST) {
    $first = $n.Split(" ")[0].ToLower()
    $last = $n.Split(" ")[1].ToLower()
    $username = "$($first.Substring(0,1))$($last)".ToLower()
    Write-Host "Creating user: $($username)" -BackgroundColor Black -ForegroundColor Cyan
    
    New-AdUser -AccountPassword $password `
               -GivenName $first `
               -Surname $last `
               -DisplayName $username `
               -Name $username `
               -EmployeeID $username `
               -PasswordNeverExpires $true `
               -Path "ou=_USERS,$(([ADSI]`"").distinguishedName)" `
               -Enabled $true
}
```
**Results:**

<img width="748" height="525" alt="Lab 89" src="https://github.com/user-attachments/assets/72a92db7-4552-406a-b0ef-cbb1315af366" /></br>

---
## SETTING UP THE CLIENT

## Step 1: `Windows 10` Virtual Machine Creation and Provisioning

The virtual machine, named `CLIENT1`, was created using the Windows 10 ISO and provisioned with 4096 MB of RAM and 1 virtual CPUs. Networker Adapter 1 was enabled on the same internal network as our domain controller.

<img width="1131" height="471" alt="Lab 99" src="https://raw.githubusercontent.com/DanielTsang26/home-lab/refs/heads/main/Screenshot%202025-08-19%20143651.png" /></br>

---

## Step 2: `Windows Setup`

Powered on the virtual machine and proceeded through the Windows 10 installation process using the ISO image.

<img width="635" height="475" alt="Lab 20" src="https://github.com/user-attachments/assets/3ed47f26-c3dd-4559-8da0-4576b3284a63" /></br>

*Windows Server 10 Pro -> Accept the license terms -> Username: dtsang -> Password: dT!181290*

---

## Step 3: Confirming `DHCP` Functionality

After setting up `CLIENT1`, I logged in using the `dtsang` account with the default credentials for the first time. I then opened a command prompt and ran `ipconfig` to verify network settings. 

<img width="538" height="280" alt="Lab 100" src="https://github.com/user-attachments/assets/d3cf4e9c-3cd4-4d3e-8dc2-33f0abed07a9" /></br>

These values confirm that the DHCP server is functioning correctly and issuing addresses within the defined scope, along with proper DNS domain suffix assignment. 

---
## Step  4: Join `CLIENT1` to the Domain

In system properties, the computer name was changed to `CLIENT1` and selected Domain under the `Member of` section. Name changes to the domain name was mydomain.com  and was prompted to provide credentials.  After the correct authentication was inputted, the system was successfully added to the domain and prompted for a restart to apply the changes.

<img width="745" height="521" alt="Lab 108" src="https://github.com/user-attachments/assets/d500ba67-c71a-42ab-819c-9d9118eb63d2" /></br>

---
## 🔑 PASSWORD AUTOMATION (RESET)

Password reset script  for automation is useful for bulk/ remote management tasks.

```powershell
# Define the username and new password
$Username = "dtsang"
$NewPasswordText = "SecureP@ssw0rd123" 

# Convert the new password to a SecureString
$NewPassword = ConvertTo-SecureString $NewPasswordText -AsPlainText -Force

# Reset the user's password
Set-ADAccountPassword -Identity $Username -NewPassword $NewPassword -Reset

# Requires the user to change password at next logon
Set-ADUser -Identity $Username -ChangePasswordAtLogon $true
```

## 🔑 PASSWORD RESET (MANUALLY)

Password reset, manually done, was in `Active Directory`,  that can be searched up in the task bar. After that, right click `_USER` to find user accounts, `dtsang`. After that, assign a fairly easy-to-remember password.

<img width="512" height="509" alt="Lab 118" src="https://raw.githubusercontent.com/DanielTsang26/home-lab/refs/heads/main/Screenshot%202025-08-19%20121433.png" /></br>

---

*This project simulates a realistic enterprise IT environment that emulates account and password provisioning, network configuration, and security policy implementation in the IT infrastructure. The involvement of this project was:*

- **`virtualization through Windows Server 2019 domain controller`**
- **`Active Directory`**
- **`DHCP`**
- **`Client Machine`**
- **`Account Provisioning`**
- **`Password Provisioning`**
- **`Network Configuration`**


