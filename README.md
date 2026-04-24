# Snort IDS Lab on Kali Linux

## Overview
This project demonstrates a hands-on Intrusion Detection System (IDS) lab built with **Snort 3** on Kali Linux. The lab focuses on configuring Snort, loading custom detection rules, generating test traffic, and validating alert output for common reconnaissance and probe activity.

## Objectives
- Install and configure Snort 3 on Kali Linux.
- Use the default `snort.lua` file as the base configuration.
- Add and test custom detection rules through `local.rules`.
- Generate traffic such as ICMP pings, TCP SYN scans, and HTTP probes.
- Capture and review alerts in fast alert format.

## Tools Used
- Kali Linux
- Snort 3
- Nmap
- Curl
- Linux networking commands
- Git and GitHub

## Project Structure
```text
snort-ids-lab/
├── README.md
├── LICENSE
├── .gitignore
├── config/
│   ├── snort.conf
│   ├── snort.lua
│   └── local.rules
├── docs/
│   ├── detection-rules.md
│   ├── log-setup.md
│   ├── testing-procedure.md
│   └── troubleshooting.md
├── logs/
│   └── sample-alert-output.txt
└── reports/
    └── project-report.md
```

## Lab Setup
The lab was configured on Kali Linux using Snort 3. Instead of creating a configuration from scratch, the default `snort.lua` file was used as the starting point and then modified to support local testing and custom rules. This matches the normal Snort 3 workflow, where the standard configuration is extended rather than replaced from a blank file [web:105][web:239].

## Configuration Approach
The Snort setup was based on:
- Default `snort.lua` as the primary configuration file.
- Custom `local.rules` for locally written detection rules.
- Rule loading through Snort’s configuration and command-line options.
- Alert review using fast alert mode, which produces compact one-line alerts [web:105][web:116][web:237].

## Example Test Rules
Example local rules used in this lab include:

```snort
alert icmp any any -> $HOME_NET any (msg:"ICMP Ping Detected"; sid:1000001; rev:1;)
alert tcp any any -> $HOME_NET 80 (flags:S; msg:"TCP SYN Scan Detected"; sid:1000002; rev:1;)
alert tcp any any -> $HOME_NET 22 (msg:"SSH Connection Attempt"; sid:1000003; rev:1;)
alert tcp any any -> $HOME_NET 80 (content:"/admin"; http_uri; msg:"Admin Page Probe Detected"; sid:1000004; rev:1;)
```

## Running Snort
Configuration testing:

```bash
sudo snort -T -c /etc/snort/snort.lua
```

Live monitoring with custom rules:

```bash
sudo snort -q -A alert_fast -i eth0 -c /etc/snort/snort.lua -R /etc/snort/rules/local.rules
```

Snort 3 supports loading local rules with the `-R` option, and fast alert mode is commonly used to produce short, readable alert lines during testing [web:105][web:239][web:116].

## Traffic Simulation
The following traffic was generated to validate the rules:

### ICMP test
```bash
ping <target-ip>
```

### SYN scan test
```bash
nmap -sS <target-ip>
```

### HTTP probe test
```bash
curl http://<target-ip>/admin
```

These tests were chosen because they are simple to reproduce and map clearly to custom Snort rules for reconnaissance and probing activity.

## Sample Alert Output
Example alert output stored in `logs/sample-alert-output.txt`:

```txt
04/24-10:12:33.123456  [**] [1:1000001:1] ICMP Ping Detected [**] [Priority: 0] {ICMP} 192.168.1.10 -> 192.168.1.4
04/24-10:12:40.556901  [**] [1:1000002:1] TCP SYN Scan Detected [**] [Priority: 1] {TCP} 192.168.1.10:53422 -> 192.168.1.4:80
04/24-10:12:45.778932  [**] [1:1000004:1] Admin Page Probe Detected [**] [Priority: 1] {TCP} 192.168.1.10:53425 -> 192.168.1.4:80
```

Fast alert mode is designed to log Snort alerts in a concise one-line format, which makes it useful for lab validation and demonstration [web:116][web:241].

## Key Learning Outcomes
This project demonstrates:
- Snort 3 installation and configuration
- Use of default `snort.lua` with targeted customization
- Writing and testing custom IDS rules
- Simulating attack traffic for validation
- Reading and documenting alert output
- Structuring a cybersecurity project for GitHub presentation

## Troubleshooting Notes
During setup, common issues included:
- No output due to traffic not matching any rule
- Wrong monitoring interface
- `local.rules` not being loaded
- Invalid or incomplete network path between the attacker and monitored host

A practical troubleshooting path is to first validate the config with `-T`, then test a simple ICMP rule, and only after that move to more specific HTTP or scan-based rules [web:105][web:112].

## Future Improvements
- Add more application-layer detection rules
- Test additional protocols such as DNS and SMB
- Integrate Snort alerts with a SIEM workflow
- Compare Snort 2 and Snort 3 rule handling
- Add packet capture replay for repeatable testing

## Author
**Shreyash Dhawale**  
Cybersecurity learner focused on network security, blue-team skills, and hands-on lab projects.
