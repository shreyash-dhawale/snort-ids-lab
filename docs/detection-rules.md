# Detection Rules

## Overview
This document explains the custom Snort 3 rules used in this lab. The rules were written for local testing and are stored in `config/local.rules`.

In Snort 3, rules define the packet conditions that must be met before an alert is generated. A rule typically contains a header and a body with rule options such as `msg`, `content`, and `sid` [cite:138][cite:132].

## Rule File Location
The repository stores local rules in:

```text
config/local.rules
```

On the running Kali system, the same rules can be placed in:

```text
/etc/snort/rules/local.rules
```

This separation helps keep the GitHub project organized while still reflecting the live runtime path used on the host system.

## Rule Syntax Basics
A Snort rule contains:
- An action, such as `alert`
- A protocol, such as `icmp` or `tcp`
- Source and destination IP/port information
- Rule options inside parentheses

Snort 3 rules rely on matching both header conditions and option-based conditions to determine whether a packet should trigger an event [cite:138][cite:245].

## Custom Rules Used

### ICMP Ping Detection
```snort
alert icmp any any -> $HOME_NET any (msg:"ICMP Ping Detected"; sid:1000001; rev:1;)
```

**Purpose:** Detect ICMP echo traffic directed toward the monitored network.  
**Why it was used:** This is the simplest test rule and is useful for validating whether Snort is seeing packets at all.

### TCP SYN Scan Detection
```snort
alert tcp any any -> $HOME_NET 80 (flags:S; msg:"TCP SYN Scan Detected"; sid:1000002; rev:1;)
```

**Purpose:** Detect TCP SYN packets targeting port 80.  
**Why it was used:** A SYN scan is a common reconnaissance technique and can be generated easily using `nmap -sS`.

### SSH Connection Attempt
```snort
alert tcp any any -> $HOME_NET 22 (msg:"SSH Connection Attempt"; sid:1000003; rev:1;)
```

**Purpose:** Detect traffic attempting to reach SSH on port 22.  
**Why it was used:** This rule helps demonstrate basic service-targeted monitoring.

### Admin Page Probe Detection
```snort
alert tcp any any -> $HOME_NET 80 (content:"/admin"; http_uri; msg:"Admin Page Probe Detected"; sid:1000004; rev:1;)
```

**Purpose:** Detect HTTP requests to the `/admin` path.  
**Why it was used:** This simulates a basic web reconnaissance or probing attempt.

Snort rule options such as `content` and protocol-specific keywords help narrow detection to specific traffic characteristics instead of matching everything broadly [cite:138][cite:245].

## Rule Design Notes
The rules in this lab were intentionally simple. The goal was not deep production-grade detection engineering, but rather to understand:
- how rules are structured,
- how traffic matches rules,
- and how alerts are generated.

Using simple rules first is an effective way to confirm that the sensor, interface, and traffic path are all working correctly before moving to more advanced detection logic.

## SID and Revision Usage
Each rule includes:
- `sid` = unique Snort rule identifier
- `rev` = rule revision number

The `sid` should be unique across local rules so that alerts can be tracked reliably and updated later if the rule changes.

## Possible Improvements
This ruleset can be extended with:
- DNS query detection
- SMB access detection
- brute-force attempt detection
- suspicious user-agent matching
- rule tuning with `flow`, `nocase`, and service-specific keywords

The Snort 3 Rule Writing Guide provides a much wider set of options for refining matches and improving rule precision [cite:245][cite:248].
