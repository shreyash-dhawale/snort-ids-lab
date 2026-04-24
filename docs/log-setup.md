# Log Setup

## Overview
This document explains how alert logging was configured for the Snort 3 lab. The goal was to produce clear and compact output that could be reviewed easily during testing and included as project evidence in the repository.

Snort supports multiple output and alerting modes. For this lab, fast alert mode was used because it writes concise one-line alerts that are suitable for demonstrations and quick testing [cite:116][cite:165].

## Logging Approach
The lab used command-line alert logging rather than a more complex external logging pipeline. This kept the setup simple and made it easier to validate whether each custom rule was working correctly.

Fast alert mode was selected because it records:
- timestamp
- alert identifier
- alert message
- protocol
- source and destination information [cite:165][cite:243]

## Validation Command
Before starting live monitoring, the configuration was tested with:

```bash
sudo snort -T -c /etc/snort/snort.lua
```

This step confirms that Snort can load the configuration successfully before traffic monitoring begins.

## Live Monitoring Command
Snort was started with the following command:

```bash
sudo snort -q -A alert_fast -i eth0 -c /etc/snort/snort.lua -R /etc/snort/rules/local.rules
```

### Option explanation
- `-q` = quiet mode
- `-A alert_fast` = use fast alert format
- `-i eth0` = listen on interface `eth0`
- `-c /etc/snort/snort.lua` = load the main Snort 3 configuration
- `-R /etc/snort/rules/local.rules` = load custom rules from the local rules file

Fast mode is designed for short one-line alert output and is commonly used in testing because it is faster and less verbose than full alert output [cite:116][cite:165].

## Sample Alert Format
Example alert lines:

```txt
04/24-10:12:33.123456  [**] [1:1000001:1] ICMP Ping Detected [**] [Priority: 0] {ICMP} 192.168.1.10 -> 192.168.1.4
04/24-10:12:40.556901  [**] [1:1000002:1] TCP SYN Scan Detected [**] [Priority: 1] {TCP} 192.168.1.10:53422 -> 192.168.1.4:80
```

This format is useful because it provides enough detail to verify detection without overwhelming the analyst with extra packet header information [cite:116][cite:243].

## Repository Evidence
A sample output file is stored in:

```text
logs/sample-alert-output.txt
```

This file represents the expected alert format for the tested rules and serves as supporting documentation for the lab.

## Future Logging Improvements
This lab can be extended by:
- writing alerts to dedicated files,
- exporting structured logs,
- forwarding alerts to a SIEM,
- or enabling more detailed output formats.

These improvements would make the project more realistic for larger-scale monitoring environments, but fast alerts were sufficient for this learning-focused lab.
