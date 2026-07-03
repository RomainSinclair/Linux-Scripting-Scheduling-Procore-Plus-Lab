# Ticket #57 — Create a Script to Gather System Information

*Category: Terminal / CLI / Scripting — Bash Script*

| **Field** | Value |
| --- | --- |
| **Ticket #** | 57 |
| **Title** | Create a Script to Gather System Information |
| **Category** | Terminal / CLI / Scripting — Bash Script |
| **Prepared by** | Romain Sinclair |
| **Environment** | Procore-Plus Lab (CentOS Stream / RHEL-based) |

## Objective

Create a Bash script that gathers key system information (date, last 10 logged-in users, swap space, kernel version, and IP address) and writes the output to /tmp/serverinfo.info on dev-app, dev-performance, and stage-web.

## Requirements

- VMs: dev-app, dev-performance, stage-web

- Gather: date, last 10 users, swap space, kernel version, IP address

- Output file: /tmp/serverinfo.info

## Environment Details

- VMs: dev-app, dev-performance, stage-web

- Output: /tmp/serverinfo.info

- Shell: Bash

## Implementation Steps

Step 1 — Create the script:

```bash
vi serverinfo.sh
```
Script content:

#!/bin/bash

{

  date

  last | head -10

  swapon --show

  uname -r

  hostname -I

} > /tmp/serverinfo.info

Step 2 — Make the script executable:

```bash
chmod +x serverinfo.sh
```
Step 3 — Run the script:

```bash
bash serverinfo.sh
```
Step 4 — Verify the output file:

```bash
cat /tmp/serverinfo.info
```
## Troubleshooting & Root Cause Analysis

The task was straightforward. The main consideration was ensuring the output path matched exactly what the ticket requested (/tmp/serverinfo.info not /tmp/serverinfo.txt).

All five data sources (date, last, swapon, uname, hostname) are redirected into a single file using { } grouping with a single > redirection — this is cleaner than multiple >> appends.

## Validation & Testing

- bash serverinfo.sh

```bash
cat /tmp/serverinfo.info
```
## Key Lessons Learned

- Use { } command grouping with a single > to redirect multiple commands to one file efficiently

- swapon --show shows active swap partitions/files; uname -r shows the kernel version

- hostname -I returns all IP addresses of the host (vs hostname -i which may return 127.0.0.1)

