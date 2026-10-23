# Network Scanning and Vulnerability Assessment

## Project Overview

This project demonstrates network reconnaissance, service enumeration, and vulnerability assessment using BlackArch Linux, Nmap, Nessus, and Metasploitable2.

Metasploitable2 is an intentionally vulnerable Linux virtual machine designed for cybersecurity education and security testing.

## Objectives

- Discover hosts on an isolated lab network.
- Identify open ports and running services.
- Detect service versions and estimate the target operating system.
- Scan the target for known vulnerabilities using Nessus.
- Analyze findings and recommend remediation measures.

## Tools and Technologies

| Tool | Purpose |
|---|---|
| Arch Linux | Host operating system |
| BlackArch Linux | Security testing machine |
| Metasploitable2 | Vulnerable target machine |
| Nmap | Port scanning and service enumeration |
| Nessus | Vulnerability assessment |
| Virtualization software | Runs the virtual machines |

## Lab Architecture

The lab contains two virtual machines connected through an isolated virtual network.

- **Scanner:** BlackArch Linux
- **Target:** Metasploitable2

Example IP addresses:

- BlackArch: `192.168.56.10`
- Metasploitable2: `192.168.56.20`

Actual addresses may differ depending on network configuration.

## Installation and Setup

### 1. Download Metasploitable2

Official sources:

- Rapid7 documentation: https://docs.rapid7.com/metasploit/metasploitable-2/
- SourceForge download: https://sourceforge.net/projects/metasploitable/files/Metasploitable2/

### 2. Extract the archive

Place the downloaded ZIP file in the project directory and run:

```bash
unzip metasploitable2.zip
```

### 3. Configure the virtual network

Connect BlackArch and Metasploitable2 to the same isolated Host-Only or Internal Network.

Do not expose Metasploitable2 to public or untrusted networks.

### 4. Identify the target IP

On Metasploitable2, log in using the default lab credentials:

- Username: `msfadmin`
- Password: `msfadmin`

Then run:

```bash
ifconfig
```

Record the IP address assigned to the network interface.

## Testing Methodology

### Phase 1: Host Discovery

Discover live systems on the lab subnet:

```bash
sudo nmap -sn 192.168.56.0/24
```

Replace the example subnet with the actual lab subnet.

### Phase 2: Port Scanning and Service Enumeration

Scan the authorized target:

```bash
nmap -sV 192.168.56.20
```

This identifies open TCP ports and attempts to determine service versions.

### Phase 3: Operating System Detection

```bash
sudo nmap -O 192.168.56.20
```

Operating system detection provides an estimate and may require additional privileges or suitable network responses.

### Phase 4: Vulnerability Assessment

1. Open the Nessus web interface.
2. Create a Basic Network Scan.
3. Enter the Metasploitable2 IP address as the target.
4. Run the scan.
5. Review findings by severity.
6. Record the evidence and recommended remediation for each important finding.

### Phase 5: Analysis and Reporting

Document the following:

- Open ports and associated services.
- Detected service versions.
- Vulnerability names and severity levels.
- Supporting evidence from the scan.
- Potential security impact.
- Recommended fixes or mitigations.

Do not treat scanner findings as confirmed vulnerabilities without reviewing the available evidence.

## Expected Outcomes

Upon completion, the project should demonstrate:

- Successful discovery of the target host.
- Identification of exposed ports and services.
- Operating system fingerprinting.
- A Nessus vulnerability assessment report.
- An analysis of security risks and remediation recommendations.

Actual findings depend on the target configuration and scanner settings.

## Security and Ethical Considerations

- Only scan systems you own or have explicit permission to test.
- Keep Metasploitable2 isolated from untrusted networks.
- Do not use the vulnerable machine for production workloads.
- Take a virtual machine snapshot before experiments.
- Store scan reports securely.

## Project Status

- [ ] Download Metasploitable2
- [ ] Extract the virtual machine files
- [ ] Configure the isolated lab network
- [ ] Verify connectivity
- [ ] Complete Nmap scans
- [ ] Run the Nessus assessment
- [ ] Analyze findings
- [ ] Complete the final report

## References

- Rapid7 Metasploitable2 Documentation: https://docs.rapid7.com/metasploit/metasploitable-2/
- Metasploitable2 on SourceForge: https://sourceforge.net/projects/metasploitable/files/Metasploitable2/
- Nmap Reference Guide: https://nmap.org/book/man.html
- Tenable Nessus Documentation: https://docs.tenable.com/nessus/

---

**Disclaimer:** This project is intended for authorized security testing and educational use within an isolated lab environment.
