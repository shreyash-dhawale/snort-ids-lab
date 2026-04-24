# Testing Procedure

## Overview
This document explains how the Snort 3 lab was tested after configuration and rule creation. The testing process was designed to verify that the sensor could load the configuration, observe live traffic, and generate alerts when packets matched the custom rules.

A good testing flow begins with validation, then moves to simple packet matches, and only after that proceeds to more specific protocol or application-layer tests [cite:132][cite:247].

## Step 1: Validate Configuration
Before generating any traffic, the Snort configuration was tested:

```bash
sudo snort -T -c /etc/snort/snort.lua
```

This step checks whether Snort can parse the configuration and initialize correctly. It helps prevent troubleshooting live traffic problems that are actually caused by configuration errors.

## Step 2: Start Snort
Snort was then started in live monitoring mode:

```bash
sudo snort -q -A alert_fast -i eth0 -c /etc/snort/snort.lua -R /etc/snort/rules/local.rules
```

This command instructed Snort to monitor traffic on `eth0`, use the main Lua configuration, and load the custom local rules.

## Step 3: Generate Test Traffic
Traffic was generated from another host or test source to trigger specific rules.

### ICMP test
```bash
ping <target-ip>
```

**Expected result:** Trigger the ICMP rule and generate an `ICMP Ping Detected` alert.

### SYN scan test
```bash
nmap -sS <target-ip>
```

**Expected result:** Trigger the SYN scan rule for TCP traffic directed at port 80.

### SSH test
```bash
nmap -p 22 <target-ip>
```

**Expected result:** Trigger the SSH connection attempt rule.

### HTTP probe test
```bash
curl http://<target-ip>/admin
```

**Expected result:** Trigger the admin page probe rule if the HTTP request reaches the monitored host and matches the `/admin` content condition.

## Step 4: Review Alert Output
While Snort was running, the console output was observed for alerts. In fast alert mode, matching packets should generate one-line alert messages showing the alert type, protocol, and endpoint details [cite:165][cite:116].

If an alert appeared, the test was considered successful. If no alert appeared, the troubleshooting process focused on:
- interface selection,
- rule matching,
- traffic path,
- and rule loading.

## Step 5: Save Evidence
Relevant output was recorded in:
- screenshots of live alerts,
- terminal output from test commands,
- and the `logs/sample-alert-output.txt` file.

This evidence supports the technical documentation and improves the project’s value as a portfolio artifact.

## Testing Strategy Notes
The testing was ordered from simplest to most specific:
1. ICMP
2. SYN scan
3. SSH
4. HTTP path match

This order matters because ICMP is usually the easiest way to confirm that Snort is seeing traffic at all. If ICMP detection fails, there is little value in testing more complex HTTP content rules until the traffic visibility problem is fixed.

## Future Testing Enhancements
Future improvements may include:
- repeatable PCAP replay testing,
- comparison between multiple interfaces,
- structured test result tables,
- and tuning rules against noisy traffic.

These additions would make the validation process more repeatable and closer to real-world detection engineering workflows.
