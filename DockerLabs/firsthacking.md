# FirstHacking

Difficulty: Very easy

Link: https://dockerlabs.es/

## Summary

- [Introduction](#introduction)
- [Exploitation](#exploitation)
- [Impact](#impact)

## Introduction

The objective of this lab is to identify and exploit a vulnerable version of the vsftpd FTP service. During the enumeration phase, it was possible to identify version 2.3.4 of the service, which is known for containing a backdoor distributed in compromised versions of the software. Exploiting this vulnerability allows remote access to the system, demonstrating the importance of properly managing and updating exposed services.

## Exploitation

After starting the lab and obtaining access to the target IP, the first step was to perform reconnaissance using Nmap to identify open ports and available services.

```bash
sudo nmap -p- -sV -sC -T3 -n -vvv -Pn 172.17.0.2 -oN escaneo
```

The parameters used were intended to perform a complete enumeration of the target:

* `-p-`: scans all available TCP ports.
* `-sV`: identifies the versions of discovered services.
* `-sC`: runs Nmap's default enumeration scripts.
* `-T3`: uses a moderate level of aggressiveness during the scan.
* `-n`: disables DNS resolution to make the process faster.
* `-vvv`: increases the verbosity level of the output.
* `-Pn`: assumes the host is up and skips the host discovery phase.

After analyzing the scan results, the following open port was identified:

```text
21/tcp open ftp vsftpd 2.3.4
```

[fh_1](/DockerLabs/img/firsthacking_1.png)

Identifying the service version is important for checking known vulnerabilities. A brief search revealed that version `vsftpd 2.3.4` contains a widely known backdoor cataloged as:

```text
CVE-2011-2523

vsftpd 2.3.4 downloaded between 20110630 and 20110703 contains a backdoor which opens a shell on port 6200/tcp.
```

This vulnerability affects compromised versions of the service distributed during a short period of time and allows a remote shell to be opened through port `6200/tcp`.

With this information, Metasploit was used to exploit the vulnerability and attempt to gain access to the system.

[fh_2](img/firsthacking_2.png)

After selecting the appropriate module, the target was configured as follows:

```text
set RHOSTS <ip>
run
```

A few seconds after executing the module, the exploitation was completed successfully. The backdoor was triggered correctly, resulting in remote access to the system with root privileges, allowing full control of the machine and completing the lab.

[fh_3](img/firsthacking_3.png)

## Impact

Exploiting CVE-2011-2523 allows remote command execution through a backdoor present in compromised versions of vsftpd 2.3.4. As demonstrated in the lab, an attacker can obtain direct access to the system with elevated privileges, completely compromising the confidentiality, integrity, and availability of the affected environment.
