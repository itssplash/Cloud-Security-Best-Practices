# Cloud Security Best Practices

## Introduction
Cloud security is essential for protecting data, applications, and cloud infrastructure from unauthorized access, breaches, and threats. This project explores various cloud security measures, including network policies, access control, and secure storage configurations.

## Objectives
- Implement **region-based policies** to restrict resource deployment locations.
- Configure **service endpoints** for private and secure storage access.
- Create **network security groups (NSGs)** to manage inbound and outbound traffic.
- Secure **cloud storage** to allow access only from designated networks.
- Deploy and test **virtual machines** to validate access control policies.

## Prerequisites
- Basic understanding of **cloud security concepts**.
- A computer with **internet access**.
- A valid **cloud account** (AWS, Azure, or GCP).
- **Web browser** to access the cloud portal.

## Lab Instructions

### **1. Create an Allowed Locations Policy**
Ensure resources are only created in a specific cloud region (e.g., Canada Central).

### **2. Configure Service Endpoints & Secure Storage**
- **Task 1:** Create a **virtual network** in the Canada Central region.
- **Task 2:** Add a **subnet** and enable a service endpoint for **Microsoft.Storage**.

### **3. Configure Network Security Groups (NSGs)**
- **Task 3:** Create an NSG to restrict access within the subnet.
  - Allow outbound traffic to the storage service.
- **Task 4:** Configure an NSG to allow **RDP access** on the public subnet.
  - Deny all other outbound internet traffic.

### **4. Secure Cloud Storage**
- **Task 5:** Create a storage account and file share.
  - Deny access from the internet.
  - Allow access only from the private subnet.

### **5. Deploy and Test Virtual Machines**
- **Task 6:** Deploy **two virtual machines** (one in private and one in public subnet).
- **Task 7:** Test access to the **storage account** from the private VM (should succeed).
- **Task 8:** Test access to the **storage account** from the public VM (should fail).

### **6. Cleanup Resources**
To prevent unexpected costs, delete all resources after completing the lab.

## Testing Commands (PowerShell)
Run the following PowerShell script inside the **private VM** to map the cloud file share:

```powershell
$key = @{
     String = "<storage-account-key>"
 }
 $acctKey = ConvertTo-SecureString @key -AsPlainText -Force

 $cred = @{
     ArgumentList = "Azure\<storage-account-name>", $acctKey
 }
 $credential = New-Object System.Management.Automation.PSCredential @cred

 $map = @{
     Name = "Z"
     PSProvider = "FileSystem"
     Root = "\\<storage-account-name>.file.core.windows.net\file-share"
     Credential = $credential
 }
 New-PSDrive @map
```

This should **successfully map** the cloud storage to drive `Z:`.

For the **public VM**, repeat the same steps. You should receive an **access denied** error.

## Conclusion
Implementing **cloud security policies, network controls, and storage protection** helps organizations safeguard their cloud environments. Proper **access control, service endpoints, and NSGs** play a vital role in securing cloud resources.

## Repository
For detailed scripts and configurations, visit: **[GitHub Project Link]**
