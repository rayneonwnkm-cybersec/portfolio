## UnrealIRCd Vulnerability Assessment & Exploitation Lab – Metasploitable 2 

## Overview

This project demonstrates a basic vulnerability assessment and controlled exploitation workflow using an intentionally vulnerable Metasploitable 2 virtual machine.

The objective was to identify exposed services, investigate a critical UnrealIRCd vulnerability, and safely test exploitation using Metasploit within an isolated virtual lab environment.

The project follows the workflow:

Network Identification → Service Enumeration → Vulnerability Identification → Vulnerability Verification → Controlled Exploitation → Shell Access

## Lab Environment

The lab was conducted using two virtual machines connected through a virtual network:

| Machine          | Purpose                           | IP Address |
| ---------------- | --------------------------------- | ---------- |
| Kali Linux       | Security testing and exploitation | `10.0.2.3` |
| Metasploitable 2 | Intentionally vulnerable target   | `10.0.2.4` |

### Network Topology

```text
┌─────────────────────┐
│     Kali Linux      │
│    10.0.2.3         │
│                     │
│ Nmap / Nessus /     │
│ Metasploit          │
└──────────┬──────────┘
           │
           │ Virtual Network
           │
┌──────────▼──────────┐
│   Metasploitable 2  │
│    10.0.2.4         │
│                     │
│ Intentionally       │
│ Vulnerable Target   │
└─────────────────────┘
```

> **Note:** The IP addresses shown above are private lab addresses used within the virtual environment.

## 1. Identify IP Addresses

The first step was to identify the IP addresses assigned to each virtual machine.

On Kali Linux, `ifconfig` was used to identify the local network configuration and confirm the Kali Linux IP address.

**Kali Linux:**

```text
10.0.2.3
```

![Kali Linux IP configuration](screenshots/01-kali-ip-configuration.png)


The Metasploitable 2 machine was identified as the target system:

```text
10.0.2.4
```

![Metasploitable 2 IP configuration](screenshots/02-metasploitable-ip-configuration.png)

These addresses were then used during the subsequent scanning and exploitation steps.

## 2. Service Enumeration with Nmap

After identifying the target IP address, Nmap was used to enumerate the services and versions exposed by the Metasploitable 2 machine.

The following command was used:

```bash
nmap -sV 10.0.2.4
```

The scan identified multiple open ports and services running on the target. The service and version information provided useful information for determining potential vulnerabilities that could be investigated further.

### Nmap Scan Results

![Nmap service enumeration](screenshots/03-nmap-scan.png)

## 3. Vulnerability Identification with Nessus

After identifying the exposed services with Nmap, Nessus was used to perform a vulnerability assessment of the Metasploitable 2 target.

The scan identified several vulnerabilities, including a **critical UnrealIRCd vulnerability**.

The UnrealIRCd finding was selected for further investigation.

### Nessus Finding

![Nessus UnrealIRCd vulnerability](screenshots/04-nessus-unrealircd-vulnerability.png)

The vulnerability identified by Nessus was then investigated using Metasploit to determine whether it could be successfully exploited in the controlled lab environment.

## 4. Controlled Exploitation with Metasploit

After Nessus identified the critical UnrealIRCd vulnerability, Metasploit was used to test the vulnerability in the controlled lab environment.

The exploitation process included:

1. Starting the Metasploit Framework with `msfconsole`.
2. Searching for the UnrealIRCd exploit module.
3. Selecting the appropriate exploit module.
4. Reviewing the available module options.
5. Setting the target IP address using `RHOSTS`.
6. Selecting a reverse shell payload.
7. Setting the Kali Linux IP address as `LHOST`.
8. Running the exploit against the Metasploitable 2 target.
9. Successfully obtaining **root access** to the Metasploitable 2 system.

### Exploitation Demonstration

The video below demonstrates the complete exploitation process, beginning with the Metasploit configuration and ending with successful root access and the reboot of the Metasploitable 2 target.

<video src="https://github.com/user-attachments/assets/9037c29d-a55c-4564-9883-52a532ff999a" controls width="800">
</video>

The demonstration was performed exclusively against the intentionally vulnerable Metasploitable 2 virtual machine in the isolated lab environment.

