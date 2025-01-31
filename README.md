
---

# **Retrieve Azure AD User OAuth2 Permission Grants using Microsoft Graph PowerShell**

## **Overview**
This PowerShell script connects to **Microsoft Graph**, retrieves a specified **Azure AD user's OAuth2 permission grants**, and lists the **applications, granted permissions, consent type, and expiry time**. It provides **error handling** for missing users, empty permission grants, and unknown applications.

This script is useful for **security audits, compliance checks, and application access reviews** in **Microsoft Entra ID (formerly Azure AD)**.

---

## **Prerequisites**
Before running this script, ensure that you have:

- **PowerShell 7+** (Recommended)
- **Microsoft Graph PowerShell SDK** installed  
- **Admin or delegated permissions** to access Azure AD data

---

## **Installation & Setup**
### **1. Install the Microsoft Graph PowerShell SDK**
If the module is not already installed, run:
```powershell
Install-Module Microsoft.Graph -Scope CurrentUser -AllowClobber -Force
```

### **2. Connect to Microsoft Graph**
The script requires the following **Graph API permissions**:
- **User.Read.All** (Read user profiles)
- **Application.Read.All** (Read applications)
- **Directory.Read.All** (Read directory data)

To authenticate and connect:
```powershell
Connect-MgGraph -Scopes "User.Read.All", "Application.Read.All", "Directory.Read.All"
```

---

## **Usage**
### **Run the Script**
1. **Open PowerShell** and ensure you have the correct permissions.
2. **Run the script** with the desired **User Principal Name (UPN)**:
   ```powershell
   $userUPN = "user@example.com"
   ```

3. **Execute the script**:
   ```powershell
   .\Get-OAuth2UserPermissions.ps1
   ```

---

## **Script Breakdown**
### **1. Connect to Microsoft Graph**
The script first checks if the Microsoft Graph module is installed and then connects:
```powershell
Connect-MgGraph -Scopes "User.Read.All", "Application.Read.All", "Directory.Read.All"
```

### **2. Retrieve User Details**
It fetches the **Azure AD user** based on the provided UPN:
```powershell
$user = Get-MgUser -UserPrincipalName $userUPN
```
If the user does not exist, the script exits.

### **3. Fetch OAuth2 Permission Grants**
Retrieves the **OAuth2 permission grants** for the user:
```powershell
$grants = Get-MgOauth2PermissionGrant -Filter "PrincipalId eq '$($user.Id)'"
```
If no grants exist, the script outputs an appropriate message.

### **4. Retrieve Associated Applications**
For each grant, the script fetches the **associated application (Service Principal)** and displays:
- **Application Name**
- **Granted Permissions (Scopes)**
- **Consent Type** (Admin/User)
- **Expiry Time**
  
---
## **Example Output**
```
Application: Microsoft Teams
Permissions: Calendars.Read, User.Read
Consent Type: User
Expiry Time: 2025-01-01T00:00:00Z
-----------------------------------
Application: SharePoint Online
Permissions: Files.Read.All
Consent Type: Admin
Expiry Time: Never
-----------------------------------
```

---

## **Error Handling**
- **User Not Found**: Exits with a message if the user does not exist.
- **No OAuth2 Grants**: Informs the user if no permission grants are found.
- **Service Principal Missing**: Displays `"Unknown Application"` if no service principal exists.

---

## **License**
This script is provided under the **MIT License**. Feel free to modify and use it as needed.

---

## **Contributing**
- Fork the repository
- Create a new branch (`feature-branch`)
- Submit a pull request

---

## **Author**
[Todd Maxey] – Security & Compliance Automation  
🚀 GitHub: [github.com/ToddMaxey]

---

