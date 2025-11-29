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
