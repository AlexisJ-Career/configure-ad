<p align="center">
<img src="https://i.imgur.com/pU5A58S.png" alt="Microsoft Active Directory Logo"/>
</p>

<h1>On-premises Active Directory Deployed in the Cloud (Azure)</h1>
This tutorial provides a detailed guide on implementing on-premises Active Directory within Azure Virtual Machines.<br />

<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Active Directory Domain Services
- PowerShell

<h2>Operating Systems Used </h2>

- Windows Server 2022
- Windows 10 (21H2)

<h2>High-Level Deployment and Configuration Steps</h2>

1. Naming the Domain Controller VM (Windows Server 2022) as "DC-1".
2. Setting the Domain Controller's NIC Private IP address to "static".
3. Allowing ICMPv4 (ping) through the Domain Controller's firewall.
4. Creating an Admin account and a User account in Active Directory.
5. Enabling "Client-1" to log in using one of the user accounts.

<h2>Deployment and Configuration Steps</h2>

<p>
  
1st: Begin by creating a resource group to contain the virtual machines: the Domain Controller (`DC-1`) and the Client Virtual Machine (`Client-1`). The Domain Controller VM will be deployed using a **Windows Server 2022** system image, a serialized copy of a system state stored in a non-volatile form.

<img src="https://i.imgur.com/yrZz8I3.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
</p>
<br />

<p>
  
2nd: Deploy the Client Virtual Machine (`Client-1`) running Windows 10 and place it in the same Resource Group and Virtual Network (Vnet) as the `DC-1` (Domain Controller).

<img src="https://i.imgur.com/Pp5DQdu.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
</p>
<br />

<p>
  
3rd: Configure the private IP address of `DC-1` as **static**, ensuring uninterrupted access for devices that need constant connectivity.

<img src="https://i.imgur.com/2UO3Wc1.png" height="60%" width="60%" alt="Disk Sanitization Steps"/>
</p>
<p>
</p>
<br />

<p>
  
4th: Verify the connection between the client device and the domain controller by accessing `Client-1` via **Remote Desktop Connection (RDP)**. Initiate a continuous ping to the private IP address of `DC-1` using the **ping -t** command. Ensure that ICMPv4 (ping) is allowed on the Domain Controller’s Windows Firewall. Once logged back into `Client-1`, confirm the successful ping operation.

>**Note**: Ensure the ICMPv4-In rules are activated when performing a continuous ping to `DC-1`.

<img src="https://i.imgur.com/7P9UgKE.png" height="60%" width="60%" alt="Disk Sanitization Steps"/>  
</p>
<p>
</p>
<br />

<p>
  
5th: Verify that the **ICMP** rule has been allowed for inbound traffic on the Windows Firewall of `DC-1` to ensure proper connectivity.

>**Note**: To access the Windows Firewall, type **wf.msc** or **firewall** in the Windows search bar.

<img src="https://i.imgur.com/K7n2bSF.png" height="60%" width="60%" alt="Disk Sanitization Steps"/>
</p>
<p>
</p>
<br />

<p>
  
6th: From `DC-1`, open the "add roles and features" option and enable Active Directory Domain Services. Promote `DC-1` as a Domain Controller in a newly created forest called `mydomain.com`. Restart Remote Desktop, and log in to `DC-1` using the credentials **"mydomain.com\labuser"**.

<img src="https://i.imgur.com/zDW6929.png" height="60%" width="60%" alt="Disk Sanitization Steps"/>
<img src="https://i.imgur.com/e14M74x.png" height="60%" width="60%" alt="Disk Sanitization Steps"/>
<img src="https://i.imgur.com/Btlmf3W.png" height="60%" width="60%" alt="Disk Sanitization Steps"/>  
<img src="https://i.imgur.com/v0CrEN2.png" height="60%" width="60%" alt="Disk Sanitization Steps"/>  
</p>
<p>
</p>
<br />

<p>
  
7th: Remaining connected to `DC-1` via Remote Desktop, create **organizational units** for admins and employees in Active Directory (AD). The newly created accounts will be placed in their respective organizational units.

To create the required folders in Active Directory, **right-click** on your domain name, navigate to the **"new"** option, select **"Organizational Unit"**, and create folders for employees, admins, and security groups.

<img src="https://i.imgur.com/L1F8cbr.png" height="60%" width="60%" alt="Disk Sanitization Steps"/>
</p>
<p>
</p>
<br />

<p>
  
8th: Create a new Organizational Unit (OU) called `_ADMINS`. Next, create an employee named "Karen Karen" with the username "**karen_admin**" and the same password. Add **"karen_admin"** to the _domain admins_ security group.

<img src="https://i.imgur.com/tq02E1F.png" height="70%" width="70%" alt="Disk Sanitization Steps"/>
<img src="https://i.imgur.com/qjiFVGj.png" height="70%" width="70%" alt="Disk Sanitization Steps"/>  
</p>
<p>
</p>
<br />

<p>
  
9th: Log out of `DC-1` for the user **mydomain.com\labuser** and log back in using the credentials **mydomain.com\karen_admin**.

<img src="https://i.imgur.com/34rZo6N.png" height="60%" width="60%" alt="Disk Sanitization Steps"/>
</p>
<p>
</p>
<br />

<p>
  
10th: Join `Client-1` to the domain **mydomain.com**. To do this, change the _DNS_ configuration on `Client-1` to the private IP address of `DC-1`. Navigate to the **Network Interface Card (NIC)** settings on `Client-1` and update the DNS configuration to use the private IP of `DC-1`.

Then, select **DNS servers** and choose the **"Custom"** option to enter the private IP address of `DC-1`.

>**Note**: Restart `Client-1` from **Azure Portal** before continuing.

<img src="https://i.imgur.com/qwU5GYy.png" height="60%" width="60%" alt="Disk Sanitization Steps"/>  
<img src="https://i.imgur.com/u9a9V9A.png" height="60%" width="60%" alt="Disk Sanitization Steps"/>   
</p>
<p>
</p>
<br />

<p>
  
11th: After updating the DNS server on `Client-1` to the private IP of `DC-1`, proceed to join `Client-1` to the domain. Navigate to **System** → **Rename This PC** → enter the domain name → click "OK". Select **Apply** and restart the system to finalize the changes.

A confirmation message will appear, stating that `Client-1` has been added to the domain.

>**Note**: You will see a notification confirming the successful addition of `Client-1` to the domain.

<img src="https://i.imgur.com/sk1jmQS.png" height="60%" width="60%" alt="Disk Sanitization Steps"/>
<img src="https://i.imgur.com/O5TlFuw.png" height="60%" width="60%" alt="Disk Sanitization Steps"/>  
</p>
<p>
</p>
<br />

<p>
  
12th: Enable *_remote desktop_* access for regular domain users on `Client-1`. Log into `Client-1` using the **domain.com\karen_admin** credentials. In the System settings, select **Remote Desktop** and click "Select Users that can remotely access this PC". Click **Add** to include users. In **Select Users or Groups**, enter "Domain Users", **Check Names** to validate the entry, and click "OK" to add users. Finally, click **Apply** to save the changes.

<img src="https://i.imgur.com/SbsFMr4.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
</p>
<br />  

13th: To create users for the `_EMPLOYEES` organizational unit in Active Directory (`DC-1`), use **PowerShell ISE** with administrative privileges. Run the provided pre-configured script to generate random employee accounts.

>**Source**: *INSERT*

<details>  
  <summary> <h6>Pre-Configured Powershell Script</h6> </summary>
  
```powershell
# ----- Edit these Variables for your own Use Case ----- #
$PASSWORD_FOR_USERS   = "Password1"
$NUMBER_OF_ACCOUNTS_TO_CREATE = 10000
# ------------------------------------------------------ #

Function generate-random-name() {
    $consonants = @('b','c','d','f','g','h','j','k','l','m','n','p','q','r','s','t','v','w','x','z')
    $vowels = @('a','e','i','o','u','y')
    $nameLength = Get-Random -Minimum 3 -Maximum 7
    $count = 0
    $name = ""

    while ($count -lt $nameLength) {
        if ($($count % 2) -eq 0) {
            $name += $consonants[$(Get-Random -Minimum 0 -Maximum $($consonants.Count - 1))]
        }
        else {
            $name += $vowels[$(Get-Random -Minimum 0 -Maximum $($vowels.Count - 1))]
        }
        $count++
    }

    return $name
}

$count = 1
while ($count -lt $NUMBER_OF_ACCOUNTS_TO_CREATE) {
    $fisrtName = generate-random-name
    $lastName = generate-random-name
    $username = $fisrtName + '.' + $lastName
    $password = ConvertTo-SecureString $PASSWORD_FOR_USERS -AsPlainText -Force

    Write-Host "Creating user: $($username)" -BackgroundColor Black -ForegroundColor Cyan
    
    New-AdUser -AccountPassword $password `
               -GivenName $firstName `
               -Surname $lastName `
               -DisplayName $username `
               -Name $username `
               -EmployeeID $username `
               -PasswordNeverExpires $true `
               -Path "ou=_EMPLOYEES,$(([ADSI]`"").distinguishedName)" `
               -Enabled $true
    $count++
}
