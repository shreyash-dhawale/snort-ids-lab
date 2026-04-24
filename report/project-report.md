# Snort IDS Lab Project Report

## Introduction
This project documents the setup, configuration, and testing of a Snort 3 Intrusion Detection System (IDS) lab on Kali Linux. The purpose of the lab was to gain practical experience with IDS deployment, custom rule creation, traffic simulation, and alert validation in a controlled environment.

Snort 3 was selected because it supports modular Lua-based configuration, flexible rule loading, and multiple alert output modes for testing and analysis [cite:105][cite:242].

## Project Objectives
The main objectives of this project were:

- Install and configure Snort 3 on Kali Linux.
- Use the default `snort.lua` file as the baseline configuration.
- Write and load custom detection rules through `local.rules`.
- Generate test traffic to trigger alerts.
- Verify alert generation using fast alert logging.
- Document the setup in a format suitable for a cybersecurity portfolio.

## Environment
The project was built in a local Kali Linux environment with Snort 3 installed as the IDS engine. Testing traffic was generated using common command-line tools such as `ping`, `nmap`, and `curl` in order to simulate reconnaissance and simple probing behavior.

The lab used Snort’s Lua-based configuration model, where rule loading and runtime behavior can be controlled through `snort.lua` and command-line options such as `-R` and `-A` [cite:105][cite:132][cite:167].

## Configuration Method
The default `snort.lua` file was used as the starting point rather than creating a blank configuration from scratch. This approach is consistent with the intended Snort 3 workflow because the standard configuration already includes the core modules and defaults needed for operation [cite:105].

Custom rules were placed in `local.rules` and loaded during execution. Snort 3 supports loading rules directly with the `-R` option, which makes it practical for small lab environments and local testing [cite:105][cite:99].

### Example execution
```bash
sudo snort -T -c /etc/snort/snort.lua
sudo snort -q -A alert_fast -i eth0 -c /etc/snort/snort.lua -R /etc/snort/rules/local.rules
```

The `-T` option was used to validate the configuration before live monitoring. Fast alert mode was selected because it produces compact one-line alerts that are easy to review during testing [cite:116][cite:165][cite:242].

## Rule Development
Several simple custom rules were created to detect common types of network activity. These rules focused on ICMP traffic, TCP SYN scanning behavior, SSH connection attempts, and HTTP requests to a sensitive path.

### Example custom rules
```snort
alert icmp any any -> $HOME_NET any (msg:"ICMP Ping Detected"; sid:1000001; rev:1;)
alert tcp any any -> $HOME_NET 80 (flags:S; msg:"TCP SYN Scan Detected"; sid:1000002; rev:1;)
alert tcp any any -> $HOME_NET 22 (msg:"SSH Connection Attempt"; sid:1000003; rev:1;)
alert tcp any any -> $HOME_NET 80 (content:"/admin"; http_uri; msg:"Admin Page Probe Detected"; sid:1000004; rev:1;)
```

These rules were intentionally simple so that their behavior could be tested and understood clearly in the lab environment. Snort rules determine whether packets are acted on, and they can be defined in rules files or through Lua-based configuration structures [cite:132][cite:105].

## Testing Procedure
Traffic was generated against the monitored host to validate whether the custom rules produced alerts.

### Test commands
```bash
ping <target-ip>
nmap -sS <target-ip>
curl http://<target-ip>/admin
```

Each command was selected because it mapped directly to one of the custom rules. ICMP traffic was used to validate basic packet matching, while `nmap -sS` and `curl` were used to simulate reconnaissance and suspicious application requests.

The testing process followed a simple progression: validate configuration, run Snort on the correct interface, generate matching traffic, and review alert output [cite:105][cite:112][cite:165].

## Alert Output
Alert logging was reviewed using fast alert mode. This format writes Snort alerts in a short one-line form, making it useful for demonstration and quick validation during lab work [cite:116][cite:165][cite:237].

### Sample output
```txt
04/24-10:12:33.123456  [**] [1:1000001:1] ICMP Ping Detected [**] [Priority: 0] {ICMP} 192.168.1.10 -> 192.168.1.4
04/24-10:12:40.556901  [**] [1:1000002:1] TCP SYN Scan Detected [**] [Priority: 1] {TCP} 192.168.1.10:53422 -> 192.168.1.4:80
04/24-10:12:45.778932  [**] [1:1000004:1] Admin Page Probe Detected [**] [Priority: 1] {TCP} 192.168.1.10:53425 -> 192.168.1.4:80
```

This output demonstrates that Snort successfully identified packets that matched the custom rules. Fast mode typically includes the timestamp, rule identifier, message, protocol, and source-destination details associated with the alert [cite:165][cite:116].

## Challenges Encountered
One of the main troubleshooting issues in the lab was the possibility of running Snort without seeing any output. In an IDS setup, a quiet console does not always mean the tool is broken; it can also mean that no packets matched the active rules [cite:105][cite:167].

Other likely causes included:
- Monitoring the wrong network interface.
- Generating traffic that did not match the rule conditions.
- Failing to load `local.rules`.
- Using application-specific rules without corresponding application traffic.

These issues were addressed by first validating the config with `-T`, then testing with a simple ICMP rule before moving to more specific scan and HTTP detection rules [cite:105][cite:112].

## Results
The lab successfully demonstrated a working Snort 3 IDS setup on Kali Linux with custom local rules and alert validation. The project showed that rule-based packet inspection can be tested effectively in a small local environment using common Linux tools and reproducible traffic patterns [cite:105][cite:132].

The project also reinforced the importance of proper interface selection, accurate rule targeting, and step-by-step troubleshooting when alerts do not appear as expected [cite:112][cite:167].

## Skills Demonstrated
This project demonstrates practical skills in:
- Network intrusion detection
- Snort 3 configuration
- Rule writing and tuning
- Linux command-line operations
- Packet-based testing and validation
- Troubleshooting IDS behavior
- Technical documentation for cybersecurity projects

## Conclusion
This Snort IDS lab provided hands-on experience with configuring and testing a signature-based detection system in Kali Linux. By using the default `snort.lua` file, adding custom rules through `local.rules`, and validating detections with fast alert logging, the project established a solid beginner-to-intermediate foundation in IDS deployment and testing [cite:105][cite:116][cite:167].

The lab can be extended further by adding more complex application-layer rules, integrating external rule sets, exporting JSON alerts, or forwarding events into a SIEM workflow for deeper analysis [cite:167][cite:238][cite:242].This Snort IDS lab provided hands-on experience with configuring and testing a signature-based detection system in Kali Linux. By using the default `snort.lua` file, adding custom rules through `local.rules`, and validating detections with fast alert logging, the project established a solid beginner-to-intermediate foundation in IDS deployment and testing [cite:105][cite:116][cite:167].

The lab can be extended further by adding more complex application-layer rules, integrating external rule sets, exporting JSON alerts, or forwarding events into a SIEM workflow for deeper analysis [cite:167][cite:238][cite:242].
