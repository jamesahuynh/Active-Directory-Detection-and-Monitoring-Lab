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
Refer to Figure 1 for the logical diagram of the Active Directory Detection and Monitoring Lab. The diagram was created using Draw.io. The lab consists of several components: a Splunk server; an Active Directory server with Splunk Universal Forwarder and Sysmon installed; a Windows 10 target machine with Splunk Universal Forwarder, Sysmon, and Atomic Red Team; a Kali Linux attacker machine; a switch; and a router.

<img width="627" height="671" alt="image" src="https://github.com/user-attachments/assets/65e2414e-e199-471a-b740-95d3f44dce8e" />

Figure 1. Active Directory Detection and Monitoring Lab Logical Diagram

### 2. Install Virtual Machines
VirtualBox was used to host the virtual machines used in the lab. The first virtual machine spun up was the Windows 10 target machine. Refer to Figure 2 for the configuration of the new Windows 10 virtual machine in VirtualBox. Windows 10 Pro was installed on the virtual machine.

<img width="1117" height="708" alt="image" src="https://github.com/user-attachments/assets/f4d69bdc-9e78-4058-a5c2-263a5c00a178" />

Figure 2. Windows 10 Target Machine Configuration

The second virtual virtual machine spun up was the Kali Linux attacker machine. A pre-built Kali Linux virtual machine was used, which can be downloaded from [kali.org](https://www.kali.org/get-kali/#kali-virtual-machines). Refer to Figure 3 for the configuration of the pre-built Kali Linux virtual machine in VirtualBox.

<img width="1210" height="895" alt="image" src="https://github.com/user-attachments/assets/8f98d4c3-0fc8-4295-bb5a-8007041ffd5f" />

Figure 3. Kali Linux Virtual Machine Configuration

The third virtual machine spun up was Windows Server 2022. Refer to Figure 4 for the configuration of the new Windows Server 2022 virtual machine in VirtualBox. Windows Server 2022 Standard Evaluation (Desktop Experience) was installed on the virtual machine.

<img width="1210" height="700" alt="image" src="https://github.com/user-attachments/assets/32400e4d-cb7c-4623-a808-33439f672a3b" />

Figure 4. Windows Server 2022 Virtual Machine Configuration

The fourth and final virtual machine spun up was an Ubuntu server that will host the Splunk server. Refer to Figure 5 for the configuration of the new Ubuntu server virtual machine in VirtualBox.

<img width="1210" height="711" alt="image" src="https://github.com/user-attachments/assets/bbf9ee06-d8fd-4a1c-bee9-1142d4e03c69" />

Figure 5. Ubuntu Server Virtual Machine Configuration
