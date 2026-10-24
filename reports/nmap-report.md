# Network Scanning and Vulnerability Assessment Report

## 1. Assessment Overview

| Item | Details |
|---|---|
| Assessment type | Network scanning and service enumeration |
| Scanner | BlackArch Linux |
| Target system | Metasploitable 2 |
| Target IP address | `192.168.122.76` |
| Network environment | Isolated virtual lab |
| Scanning tool | Nmap 7.992 |
| Assessment scope | Connectivity, TCP ports, service versions, OS detection, default scripts |

## 2. Connectivity Verification

**Command**
```bash
ping -c 4 192.168.122.76
```

**Results**
- Target host was reachable.
- Network connectivity between the scanner and target was confirmed.

## 3. TCP Port Scanning

**Command**
```bash
nmap 192.168.122.76
```

**Results**
- Target host was active.
- 23 open TCP ports were identified.
- 977 closed TCP ports were reported.
- The scan accounted for 1,000 TCP ports.

## 4. Service and Version Detection

**Command**
```bash
nmap -sV --version-light --reason -oN scans/03-service-detection.txt 192.168.122.76
```

**Results**

| Port | State | Service | Detected version |
|---:|---|---|---|
| 21/tcp | Open | FTP | vsftpd 2.3.4 |
| 22/tcp | Open | SSH | OpenSSH 4.7p1 Debian 8ubuntu1 |
| 23/tcp | Open | Telnet | Linux telnetd |
| 25/tcp | Open | SMTP | Postfix smtpd |
| 53/tcp | Open | DNS | ISC BIND 9.4.2 |
| 80/tcp | Open | HTTP | Apache httpd 2.2.8 |
| 111/tcp | Open | RPC | rpcbind |
| 139/tcp | Open | NetBIOS | Samba 3.X–4.X |
| 445/tcp | Open | SMB | Samba 3.X–4.X |
| 512/tcp | Open | Remote exec | Version unidentified |
| 513/tcp | Open | Remote login | Service detected |
| 514/tcp | Open | Remote shell | Version unidentified |
| 1099/tcp | Open | Java RMI | GNU Classpath grmiregistry |
| 1524/tcp | Open | Bindshell | Metasploitable root shell |
| 2049/tcp | Open | RPC/NFS-related | rpcbind reported |
| 2121/tcp | Open | FTP | ProFTPD 1.3.1 |
| 3306/tcp | Open | MySQL | MySQL 5.0.51a-3ubuntu5 |
| 5432/tcp | Open | PostgreSQL | PostgreSQL 8.3.0–8.3.7 |
| 5900/tcp | Open | VNC | Protocol 3.3 |
| 6000/tcp | Open | X11 | Access denied |
| 6667/tcp | Open | IRC | UnrealIRCd |
| 8009/tcp | Open | AJP | Apache Jserv Protocol 1.3 |
| 8180/tcp | Open | HTTP | Apache Tomcat/Coyote JSP engine 1.1 |

**Additional observations**
- Nmap identified the host as `metasploitable.localdomain`.
- The scan identified Unix/Linux as the operating system family.
- Service identification for some ports was incomplete.

## 5. Operating System Detection

**Command**
```bash
nmap -O --osscan-limit -oN scans/04-os-detection.txt 192.168.122.76
```

**Results**
- Device type: General purpose.
- Operating system family: Linux.
- Estimated kernel range: Linux 2.6.9–2.6.33.
- Network distance: One hop.
- MAC address: `52:54:00:04:5E:C2` (QEMU virtual NIC).

The operating system identification is an Nmap estimate based on network responses.

## 6. Default Script Scan

**Command**
```bash
nmap -sV -sC --reason -oN scans/04-default-scripts.txt 192.168.122.76
```

**Results**

| Port | Service / version | Script-specific findings |
|---:|---|---|
| 21/tcp | FTP — vsftpd 2.3.4 | Anonymous FTP login succeeded with response code 230. The service does not encrypt FTP traffic by default. |
| 22/tcp | SSH — OpenSSH 4.7p1 | DSA 1024-bit and RSA 2048-bit host keys were reported. |
| 23/tcp | Telnet | Telnet service detected. No additional port-specific script finding confirmed in the supplied output. |
| 25/tcp | SMTP — Postfix | SMTP commands were enumerated. SSLv2 support was detected. The reported certificate validity period was 17 March–16 April 2010. |
| 53/tcp | DNS — BIND 9.4.2 | DNS service and version identified. No additional DNS script finding confirmed in the supplied output. |
| 80/tcp | HTTP — Apache 2.2.8 | HTTP title: `Metasploitable2 - Linux`. |
| 111/tcp | RPC — rpcbind | RPC service detected. The `rpcinfo` script did not return a successful result. |
| 139/tcp | NetBIOS/Samba | NetBIOS name `METASPLOITABLE` and workgroup `WORKGROUP` were reported. |
| 445/tcp | SMB — Samba 3.0.20-Debian | SMB OS discovery identified Unix/Samba 3.0.20-Debian. SMB message signing was reported as disabled. SMB2 time negotiation failed. |
| 512/tcp | Remote exec | Service detected. No additional port-specific script finding confirmed in the supplied output. |
| 513/tcp | Remote login | Remote login service detected. No additional port-specific script finding confirmed in the supplied output. |
| 514/tcp | Remote shell | Service detected, but version identification was unsuccessful. |
| 1099/tcp | Java RMI | GNU Classpath registry identified. No additional port-specific script finding confirmed in the supplied output. |
| 1524/tcp | Bindshell | Service fingerprint identified a `Metasploitable root shell`. This is a high-priority exposure in the lab target. |
| 2049/tcp | RPC/NFS-related | RPC-related service detected. The `rpcinfo` script did not return a successful result. No additional NFS script finding confirmed. |
| 2121/tcp | FTP — ProFTPD 1.3.1 | FTP service and version identified. No additional port-specific script finding confirmed in the supplied output. |
| 3306/tcp | MySQL 5.0.51a | MySQL service and protocol information identified. No additional database enumeration finding confirmed in the supplied output. |
| 5432/tcp | PostgreSQL 8.3.x | PostgreSQL service identified. The reported certificate validity period was 17 March–16 April 2010. |
| 5900/tcp | VNC — protocol 3.3 | VNC authentication was reported as required. |
| 6000/tcp | X11 | X11 service detected; access was denied. |
| 6667/tcp | IRC — UnrealIRCd 3.2.8.1 | IRC service and version identified. |
| 8009/tcp | AJP 1.3 | AJP service detected. The script failed to obtain a valid response. |
| 8180/tcp | HTTP — Apache Tomcat 5.5 | HTTP title: `Apache Tomcat/5.5`. |

### Host-Level Script Findings

- **SMB host discovery:** Hostname `metasploitable`; FQDN `metasploitable.localdomain`; domain `localdomain`.
- **SMB security mode:** The script reported guest account use and disabled message signing.
- **Clock skew:** The script reported a mean skew of approximately 59 minutes and 59 seconds, a deviation of 2 hours, and a median of 0.
- **Script errors:** Several scripts, including `rpcinfo` and some HTTP-related checks, returned errors or incomplete results. These outcomes are inconclusive and are not treated as confirmed vulnerabilities.

## 7. Security Findings and Risk Assessment

| Finding | Evidence | Priority |
|---|---|---|
| Exposed root shell | Port 1524 identified as a Metasploitable root shell | Critical |
| Anonymous FTP access | Anonymous login succeeded on port 21 | High |
| Legacy encryption protocol | SSLv2 support detected on port 25 | High |
| Unencrypted remote-access protocols | FTP and Telnet services exposed | High |
| SMB message signing disabled | SMB script result on port 445 | Medium |
| Legacy software versions | Multiple older service versions identified | High — further validation required |
| Exposed database services | MySQL on port 3306 and PostgreSQL on port 5432 | Medium — depends on access controls and intended exposure |
| Additional remote-access and application services | Ports 512, 513, 514, 1099, 5900 and 8180 among others | Further investigation required |

*Priorities are preliminary assessments based on the supplied scan results, not results from a dedicated vulnerability scanner. The presence of an older version does not by itself confirm exploitability.*

## 8. Recommendations

1. Disable or restrict the root-shell service on port 1524 outside the intentionally vulnerable lab.
2. Disable anonymous FTP access unless explicitly required.
3. Replace Telnet and plain FTP with secure alternatives, such as SSH and SFTP.
4. Disable SSLv2 and configure supported modern TLS protocols.
5. Enable SMB message signing where appropriate.
6. Restrict database and remote-access ports to trusted hosts.
7. Verify identified software versions and assess them against applicable vulnerability advisories.
8. Repeat scanning after changes to confirm that exposed services and insecure configurations have been addressed.

## 9. Conclusion

The Nmap assessment identified 23 open TCP ports on the Metasploitable 2 target. Service and version detection, operating system fingerprinting, and default script checks revealed several security concerns, including anonymous FTP access, SSLv2 support, disabled SMB message signing, legacy software, and a service identified as a root shell.

These findings provide a basis for further vulnerability validation and remediation within the authorized lab environment. Script errors and incomplete responses were treated as inconclusive rather than confirmed vulnerabilities.

## 10. Evidence Files

- `scans/03-service-detection.txt`
- `scans/04-os-detection.txt`
- `scans/04-default-scripts.txt`

**Assessment scope:** Network scanning and service enumeration of an isolated virtual lab target.