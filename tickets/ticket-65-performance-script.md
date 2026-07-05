# Ticket #65 — Run Performance Script

*Category: Terminal / CLI / Scripting — Bash Script*

| **Field** | Value |
| --- | --- |
| **Ticket #** | 65 |
| **Title** | Run Performance Script |
| **Category** | Terminal / CLI / Scripting — Bash Script |
| **Prepared by** | Romain Sinclair |
| **Environment** | Procore-Plus Lab (CentOS Stream / RHEL-based) |

## Objective

Run /nfs/incoming/scripts/performance.sh as root on dev-app, dev-performance, and stage-web. The script is expected to create /setup_file.cfg on each server.

## Requirements

- VMs: dev-app, dev-performance, stage-web

- Run /nfs/incoming/scripts/performance.sh as root

- Verify /setup_file.cfg is created after execution

## Environment Details

- VMs: dev-app, dev-performance, stage-web

- Script: /nfs/incoming/scripts/performance.sh

- Output: /setup_file.cfg

## Implementation Steps

Step 1 — Verify the script exists and is accessible:

```bash
ls -l /nfs/incoming/scripts/performance.sh
```
Step 2 — Escalate to root:

```bash
sudo -i
```
Step 3 — Run the performance script:

/nfs/incoming/scripts/performance.sh

Step 4 — Verify the output file was created:

```bash
ls -l /setup_file.cfg
```
stat /setup_file.cfg

## Troubleshooting & Root Cause Analysis

The task was operationally simple. The key requirement was to make sure the script was run as root (not just with sudo) since /setup_file.cfg is created at the filesystem root and requires root write access.

If the script is not executable, run chmod +x /nfs/incoming/scripts/performance.sh first, or execute it with bash /nfs/incoming/scripts/performance.sh.

## Validation & Testing

```bash
ls -l /setup_file.cfg
```
stat /setup_file.cfg

## Key Lessons Learned

- Some scripts must be run as root (not sudo) — use sudo -i to get a full root shell

- stat provides richer file metadata than ls — use it to confirm file creation timestamp and owner

- Always verify the script exists and is readable from the NFS share before running

