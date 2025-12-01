# Active Directory Detection and Monitoring Lab

## Objective

The Active Directory Detection and Monitoring Lab aimed to establish a controlled environment for simulating and detecting cyber attacks. The primary focus was to ingest and analyze logs within a Security Information and Event Management (SIEM) system, generating test telemetry to mimic real-world attack scenarios. This hands-on experience was designed to deepen understanding of network security, attack patterns, and defensive strategies.

### Skills Learned

- Advanced understanding of SIEM concepts and their practical application
- Proficiency in analyzing and interpreting network logs
- Ability to generate and recognize attack signatures and patterns
- Enhanced knowledge of network protocols and security vulnerabilities
- Development of critical thinking and problem-solving skills in cybersecurity

### Tools Used

- Splunk, a Security Information and Event Management (SIEM) system, for log ingestion and analysis
- Active Directory as an environment to manage resources, such as users, computers, and groups
- Splunk Universal Forwarder to collect and forward telemetry to the Splunk server for indexing and searching
- Sysmon to generate telemetry, which was forwarded to the Splunk server via Splunk Universal Forwarder
- Atomic Red Team to simulate adversarial activity in the lab environment

## Steps
### 1. Creating a Logical Diagram
Refer to Figure 1 for the logical diagram of the Active Directory Detection and Monitoring Lab. The diagram was created using Draw.io. The lab consists of several components: a Splunk server; an Active Directory server with Splunk Universal Forwarder and Sysmon installed; a Windows 10 target machine with Splunk Universal Forwarder, Sysmon, and Atomic Red Team installed; a Kali Linux attacker machine; a switch; and a router.

<img width="627" height="671" alt="image" src="https://github.com/user-attachments/assets/65e2414e-e199-471a-b740-95d3f44dce8e" />

Figure 1. Active Directory Detection and Monitoring Lab Logical Diagram

### 2. Installing Virtual Machines
VirtualBox was used to host the virtual machines used in the lab. The first virtual machine spun up was the Windows 10 target machine. Refer to Figure 2 for the configuration of the new Windows 10 virtual machine in VirtualBox. Windows 10 Pro was installed on the virtual machine.

<img width="1117" height="708" alt="image" src="https://github.com/user-attachments/assets/f4d69bdc-9e78-4058-a5c2-263a5c00a178" />

Figure 2. Windows 10 Target Machine Configuration

The second virtual machine spun up was the Kali Linux attacker machine. A pre-built Kali Linux virtual machine was used, which can be downloaded from [kali.org](https://www.kali.org/get-kali/#kali-virtual-machines). Refer to Figure 3 for the configuration of the pre-built Kali Linux virtual machine in VirtualBox.

<img width="1210" height="895" alt="image" src="https://github.com/user-attachments/assets/8f98d4c3-0fc8-4295-bb5a-8007041ffd5f" />

Figure 3. Kali Linux Virtual Machine Configuration

The third virtual machine spun up was Windows Server 2022. Refer to Figure 4 for the configuration of the new Windows Server 2022 virtual machine in VirtualBox. Windows Server 2022 Standard Evaluation (Desktop Experience) was installed on the virtual machine.

<img width="1210" height="700" alt="image" src="https://github.com/user-attachments/assets/32400e4d-cb7c-4623-a808-33439f672a3b" />

Figure 4. Windows Server 2022 Virtual Machine Configuration

The fourth and final virtual machine spun up was an Ubuntu server that will host the Splunk server. Refer to Figure 5 for the configuration of the new Ubuntu server virtual machine in VirtualBox.

<img width="1210" height="711" alt="image" src="https://github.com/user-attachments/assets/bbf9ee06-d8fd-4a1c-bee9-1142d4e03c69" />

Figure 5. Ubuntu Server Virtual Machine Configuration

### 3. Install and Configure Sysmon and Splunk Universal Forwarder

Sysmon and Splunk Universal Forwarder were installed on the Windows 10 target machine and Windows Server 2022 to collect telemetry and send logs to the Splunk server. In VirtualBox, a NAT network was created so that the virtual machines in the lab could be in the same network and still have internet access. Refer to Figure 6 for the NAT network created in VirtualBox for the lab.

<img width="983" height="813" alt="image" src="https://github.com/user-attachments/assets/c3faa764-63a0-4406-a431-54347c6dbf02" />

Figure 6. Lab NAT Network

Each of the virtual machines in the lab were attached to the newly created NAT network via their network settings. On the Splunk server, the 'ip a' command was run to confirm the assigned IP address. Refer to Figure 7 for the result of the 'ip a' command on the Splunk server.

<img width="854" height="649" alt="image" src="https://github.com/user-attachments/assets/bb000d2e-702c-4444-a282-fb719e65ae0b" />

Figure 7. Splunk Server

The Splunk server was assigned an IP address of 192.168.10.4. From the logical diagram in Figure 1, the Splunk server was assigned a static IP address of 192.168.10.10. To set a static IP address of 192.168.10.10 on the Splunk server, the 'sudo nano /etc/netplan/50-cloud-init.yaml' command was run. Within the network configuration, the dhcp4 setting was set to 'no'. In addition, an entry was created to assign a static IP address of 192.168.10.10 along with entries for the nameservers and routes. Refer to Figure 8 for the updated network configuration for the Splunk server. To apply the updated network configuration, the 'sudo netplan apply' command was run on the Splunk server.

<img width="854" height="271" alt="image" src="https://github.com/user-attachments/assets/fdb87595-f97e-4784-ba07-8ac4dfdc2487" />

Figure 8. Updated Splunk Server Network Configuration

To begin installing Splunk on the Splunk server virtual machine, Splunk Enterprise was downloaded from [splunk.com](https://www.splunk.com/) onto the host machine after creating an account. On the Splunk server virtual machine, guest additions for VirtualBox were installed. In VirtualBox, a shared folder was created so that the Splunk server virtual machine could access the Splunk Enterprise installer located on the host machine. After rebooting the Splunk server, the user was added to the vboxsf group by running the 'sudo adduser splunk vboxsf' command. On the Splunk server, a new directory called 'share' was created. With the newly created directory, the 'sudo mount -t vboxsf -o uid=1000,gid=1000 Downloads share/' command was run to mount the shared folder in VirtualBox onto the directory. After moving into the 'share' directory, the installation of Splunk Enterprise was started. To change into the directory where the Splunk binaries were located, the 'cd /opt/splunk/bin' command was run. The './splunk start' command was run to initiate the Splunk Enterprise installer. In the installer, an Administrator account was created. With the installation completed, the 'sudo ./splunk enable boot-start -user splunk' command was run so that Splunk will start every time the Splunk server virtual machine reboots. 

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
