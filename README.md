# Active Directory Detection and Monitoring Lab

## Objective
The Active Directory Detection and Monitoring Lab aimed to establish a controlled environment for simulating and detecting cyber attacks. The primary focus was to ingest and analyze logs within a Security Information and Event Management (SIEM) system, generating test telemetry to mimic real-world attack scenarios. This hands-on experience was designed to deepen understanding of network security, attack patterns, and defensive strategies.

## Skills Learned
- Advanced understanding of SIEM concepts and their practical applications
- Proficiency in analyzing and interpreting network logs
- Ability to generate and recognize attack signatures and patterns
- Enhanced knowledge of network protocols and security vulnerabilities
- Development of critical thinking and problem-solving skills in cybersecurity

## Tools Used
- Splunk, a Security Information and Event Management (SIEM) system, for log ingestion and analysis
- Active Directory as an environment to manage resources, such as users, computers, and groups
- Splunk Universal Forwarder to collect and forward telemetry to the Splunk server for indexing and searching
- Sysmon to generate telemetry, which was forwarded to the Splunk server via Splunk Universal Forwarder
- Kali Linux to perform a brute force attack
- Atomic Red Team to simulate adversarial activity in the lab environment

## Steps
### 1. Creating a Logical Diagram
Refer to Figure 1 for the logical diagram of the Active Directory Detection and Monitoring Lab. The diagram was created using Draw.io. The lab consists of several components: a Splunk server; an Active Directory server with Splunk Universal Forwarder and Sysmon installed; a Windows 10 target machine with Splunk Universal Forwarder, Sysmon, and Atomic Red Team installed; a Kali Linux attacker machine; a switch; and a router.

<img width="627" height="671" alt="image" src="https://github.com/user-attachments/assets/65e2414e-e199-471a-b740-95d3f44dce8e" />

Figure 1. Active Directory Detection and Monitoring Lab Logical Diagram

### 2. Installing Virtual Machines
VirtualBox was used to host the virtual machines used in the lab. The first virtual machine created was the Windows 10 target machine. Refer to Figure 2 for the configuration of the new Windows 10 virtual machine in VirtualBox. Windows 10 Pro was installed on the virtual machine.

<img width="1117" height="708" alt="image" src="https://github.com/user-attachments/assets/f4d69bdc-9e78-4058-a5c2-263a5c00a178" />

Figure 2. Windows 10 Target Machine Configuration

The second virtual machine created was the Kali Linux attacker machine. A pre-built Kali Linux virtual machine was used, which can be downloaded from [kali.org](https://www.kali.org/get-kali/#kali-virtual-machines). Refer to Figure 3 for the configuration of the pre-built Kali Linux virtual machine in VirtualBox.

<img width="1210" height="895" alt="image" src="https://github.com/user-attachments/assets/8f98d4c3-0fc8-4295-bb5a-8007041ffd5f" />

Figure 3. Kali Linux Virtual Machine Configuration

The third virtual machine created was Windows Server 2022. Refer to Figure 4 for the configuration of the new Windows Server 2022 virtual machine in VirtualBox. Windows Server 2022 Standard Evaluation (Desktop Experience) was installed on the virtual machine.

<img width="1210" height="700" alt="image" src="https://github.com/user-attachments/assets/32400e4d-cb7c-4623-a808-33439f672a3b" />

Figure 4. Windows Server 2022 Virtual Machine Configuration

The fourth and final virtual machine created was an Ubuntu server that will host the Splunk server. Refer to Figure 5 for the configuration of the new Ubuntu server virtual machine in VirtualBox.

<img width="1210" height="711" alt="image" src="https://github.com/user-attachments/assets/bbf9ee06-d8fd-4a1c-bee9-1142d4e03c69" />

Figure 5. Ubuntu Server Virtual Machine Configuration

### 3. Installing and Configuring Sysmon and Splunk Universal Forwarder
Sysmon and Splunk Universal Forwarder were installed on the Windows 10 target machine and Windows Server 2022 to collect telemetry and send logs to the Splunk server. In VirtualBox, a NAT network was created so that the virtual machines in the lab could be in the same network and still have internet access. Refer to Figure 6 for the NAT network created in VirtualBox for the lab.

<img width="983" height="813" alt="image" src="https://github.com/user-attachments/assets/c3faa764-63a0-4406-a431-54347c6dbf02" />

Figure 6. Lab NAT Network

Each of the virtual machines in the lab were attached to the newly created NAT network via their network settings. On the Splunk server, the 'ip a' command was run to confirm the assigned IP address. Refer to Figure 7 for the result of the 'ip a' command on the Splunk server.

<img width="854" height="649" alt="image" src="https://github.com/user-attachments/assets/bb000d2e-702c-4444-a282-fb719e65ae0b" />

Figure 7. Splunk Server

The Splunk server was assigned an IP address of 192.168.10.4. From the logical diagram in Figure 1, the Splunk server was assigned a static IP address of 192.168.10.10. To set a static IP address of 192.168.10.10 on the Splunk server, the 'sudo nano /etc/netplan/50-cloud-init.yaml' command was run. Within the network configuration, the dhcp4 setting was set to 'no'. In addition, an entry was created to assign a static IP address of 192.168.10.10 along with entries for the nameservers and routes. Refer to Figure 8 for the updated network configuration for the Splunk server. To apply the updated network configuration, the 'sudo netplan apply' command was run on the Splunk server.

<img width="854" height="271" alt="image" src="https://github.com/user-attachments/assets/fdb87595-f97e-4784-ba07-8ac4dfdc2487" />

Figure 8. Updated Splunk Server Network Configuration

To begin installing Splunk on the Splunk server virtual machine, Splunk Enterprise was downloaded from [splunk.com](https://www.splunk.com/) onto the host machine after creating an account. On the Splunk server virtual machine, VirtualBox Guest Additions were installed. In VirtualBox, a shared folder was created so that the Splunk server virtual machine could access the Splunk Enterprise installer located on the host machine. After rebooting the Splunk server, the user was added to the vboxsf group by running the 'sudo adduser splunk vboxsf' command. On the Splunk server, a new directory called 'share' was created. With the newly created directory, the 'sudo mount -t vboxsf -o uid=1000,gid=1000 Downloads share/' command was run to mount the shared folder in VirtualBox onto the directory. After moving into the 'share' directory, the installation of Splunk Enterprise was started. To change into the directory where the Splunk binaries were located, the 'cd /opt/splunk/bin' command was run. The './splunk start' command was run to initiate the Splunk Enterprise installer. In the installer, an Administrator account was created. With the installation completed, the 'sudo ./splunk enable boot-start -user splunk' command was run so that Splunk will start every time the Splunk server virtual machine reboots. 

On the Windows 10 target machine, Splunk Universal Forwarder was installed. The Windows 10 target machine was renamed to 'target-PC' and rebooted. In the command prompt, the 'ipconfig' command was run to confirm the assigned IP address. The Windows 10 target machine was assigned an IP address of 192.168.10.5. In the network settings, a static IP address of 192.168.10.100 was assigned to the Windows 10 target machine. Refer to Figure 9 for the updated Windows IP configuration of the Windows 10 target machine.

<img width="605" height="183" alt="image" src="https://github.com/user-attachments/assets/047c2593-14cc-4bc1-9936-a22c378c58e4" />

Figure 9. Updated Windows IP Configuration

After assigning the static IP address to the Windows 10 target machine, the Edge browser was opened and the Splunk server was accessed at 192.168.10.10:8000. Refer to Figure 10 for the Splunk Enterprise login screen. 

<img width="885" height="542" alt="image" src="https://github.com/user-attachments/assets/e8eefb4d-c7b7-4c20-aaa8-c6973d0ac4f2" />

Figure 10. Splunk Enterprise Login Screen

Splunk Universal Forwarder was downloaded from [splunk.com](https://www.splunk.com/) onto the Windows 10 target machine. The installer was started and an Administrator account was created. The receiving indexer was set to 192.168.10.10 at port 9997. With Splunk Universal Forwarder installed, Sysmon was installed next. After downloading Sysmon, the sysmonconfig.xml file was downloaded from [here](https://github.com/olafhartong/sysmon-modular/blob/master/sysmonconfig.xml). In PowerShell with administrative privileges, Sysmon was installed onto the Windows 10 target machine using the downloaded configuration file. To instruct Splunk Universal Forwarder on what is required to be sent to the Splunk server, the inputs.conf file was edited. Splunk Universal Forwarder was instructed to send events related to Application, Security, System, and Sysmon to the Splunk server. Refer to Figure 11 for the updated inputs.conf file.

<img width="600" height="317" alt="image" src="https://github.com/user-attachments/assets/31fa79d0-d502-4aee-9cff-98263fa637f7" />

Figure 11. inputs.conf File

With the inputs.conf file updated, the Splunk Universal Forwarder service was configured to log in as the local account and restarted. In Splunk, a new index called 'endpoint' was created. To enable the Splunk server to receive data, the receiving port was set to 9997. In the 'Search & Reporting' app in Splunk, 'index=endpoint' was entered in the search bar. Refer to Figure 12 for the event log sources starting to enter Splunk.

<img width="499" height="314" alt="image" src="https://github.com/user-attachments/assets/a940122c-2610-48d8-aed3-04678f13c29e" />

Figure 12. Splunk Event Log Sources

Splunk Universal Forwarder and Sysmon were installed on the Windows Server 2022 virtual machine using the steps followed above. In addition, the Windows Server 2022 virtual machine was renamed to 'ADDC01' and assigned a static IP address of 192.168.10.7. The Windows Server 2022 virtual machine was rebooted to apply the changes.

### 4. Installing and Configuring Active Directory
In the Server Manager on the Windows Server 2022 virtual machine, the 'Manage' button was clicked to navigate to 'Add Roles and Features'. 'Role-based or feature-based installation' was selected as the installation type. The 'ADDC01' Windows Server 2022 virtual machine with an IP address of 192.168.10.10 was selected as the destination server. Active Directory Domain Services was selected to be installed onto the selected server. Once the installation was completed, the 'ADDC01' server was promoted to a domain controller. In the Active Directory Domain Services Configuration Wizard, 'Add a new forest' was selected as the deployment operation. The root domain name was set to 'myaddomain.local'. The rest of the configuration wizard was completed using the default options. After the installation was completed, the server was automatically rebooted. 

In the Server Manager, the 'Tools' button was clicked to navigate to 'Active Directory Users and Computers'. A new organizational unit called 'IT' was created. Within the organizational unit, a new user named 'Jenny Smith' with a logon name of 'jsmith' was created. Another organizational unit called 'HR' was created with a new user named 'Terry Smith' with logon name 'tsmith'. 

On the Windows 10 target machine, the preferred DNS server was set to the AD domain controller at the IP address 192.168.10.7. The Windows 10 target machine was joined to the 'myaddomain.local' domain using the Administrator account of the AD domain controller. Once the Windows 10 target machine was rebooted, jsmith's account was used to log in. 

### 5. Performing a Brute Force Attack
On the Kali Linux attacker machine, a static IP address of 192.168.10.250 was assigned. A new directory called 'ad-project' was created on the desktop by running the command 'mkdir ad-project'. The tool Crowbar was installed by running the command 'sudo apt-get install -y crowbar'. Crowbar will be used to perform brute force attacks on the AD domain controller and Windows 10 target machine. The popular wordlist that comes installed with Kali Linux called rockyou.txt will be used for the brute force attacks. The rockyou.txt file was copied into the 'ad-project' folder on the desktop. A new file called passwords.txt was created to only capture the first 20 lines of the rockyou.txt file. This was performed by running the command 'head -n 20 rockyou.txt > passwords.txt'. Refer to Figure 13 for the contents of the passwords.txt file. The passwords of the users in the 'myaddomain.local' domain were added to the passwords.txt file.

<img width="345" height="416" alt="image" src="https://github.com/user-attachments/assets/8acdeb27-d6b2-4ad9-b078-4456e984af2c" />

Figure 13. passwords.txt 

On the Windows 10 target machine, Remote Desktop was enabled to allow remote connections to the machine. The accounts 'jsmith' and 'tsmith' were added as Remote Desktop users. On the Kali Linux attacker machine, the brute force attack on tsmith's account was initiated by running the command 'crowbar -b rdp -u tsmith -C passwords.txt -s 192.168.10.100/32'. Crowbar will try every password listed in the passwords.txt file. Refer to Figure 14 for the output from Crowbar.

<img width="526" height="44" alt="image" src="https://github.com/user-attachments/assets/b86d7061-5330-444d-9122-52a30244d404" />

Figure 14. Crowbar Output

The telemetry generated from the brute force attack on tsmith's account was analyzed in Splunk using the query 'index=endpoint tsmith'. Upon inspection of the EventCode field, there were 20 of 23 events with an event code of 4625. Reference Figure 15 for the information from the EventCode field.  

<img width="503" height="339" alt="image" src="https://github.com/user-attachments/assets/12ad47fa-acd7-4723-97d1-8ff30432e5ed" />

Figure 15. EventCode Field

An event code of 4625 indicates that an account failed to log on. There is a single event with an event code of 4624, indicating that an account was successfully logged on. 

### 6. Installing and Running Atomic Red Team
On the Windows 10 target machine, PowerShell was run as Administrator. The 'Set-ExecutionPolicy Bypass CurrentUser' command was run. An exclusion for the entire C:\ drive was performed to exclude the files from Atomic Red Team from Windows Defender. To install Atomic Red Team, the command 'IEX (IWR 'https://raw.githubusercontent.com/redcanaryco/invoke-atomicredteam/master/install-atomicredteam.ps1' -UseBasicParsing); Install-AtomicRedTeam' was run. The command 'Invoke-AtomicTest T1136.001' was run to generate telemetry based on creating a local account. A new user with the username 'NewLocalUser' was created. In Splunk, a new search with the query 'index=endpoint NewLocalUser' was performed. No events were available, which indicates that Splunk is blind to this activity. If an attacker compromised the system with the current configuration and created a local account, Splunk would not be able to detect that activity. 
