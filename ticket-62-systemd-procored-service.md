# Ticket #62 — Create a Systemd Service for procored

*Category: Terminal / CLI / Scripting — System Management*

| **Field** | Value |
| --- | --- |
| **Ticket #** | 62 |
| **Title** | Create a Systemd Service for procored |
| **Category** | Terminal / CLI / Scripting — System Management |
| **Prepared by** | Romain Sinclair |
| **Environment** | Procore-Plus Lab (CentOS Stream / RHEL-based) |

## Objective

Deploy the procored daemon to dev-app, dev-performance, and stage-web by copying it from the NFS share to /usr/bin/, creating a systemd unit file, and starting the service on all three servers.

## Requirements

- VMs: dev-app, dev-performance, stage-web

- Binary: /nfs/incoming/scripts/procored -> /usr/bin/procored

- Create systemd service unit: /etc/systemd/system/procored.service

- Enable and start procored on all three servers

## Environment Details

- VMs: dev-app, dev-performance, stage-web

- Binary path: /usr/bin/procored

- Unit file: /etc/systemd/system/procored.service

## Implementation Steps

Step 1 — Copy the binary from the NFS share:

```bash
sudo cp /nfs/incoming/scripts/procored /usr/bin/procored
```
Step 2 — Make it executable:

```bash
sudo chmod 755 /usr/bin/procored
```
Step 3 — Create the systemd unit file:

```bash
sudo vi /etc/systemd/system/procored.service
```
[Unit]

Description=Procored Daemon

After=network.target

[Service]

ExecStart=/usr/bin/procored

Restart=on-failure

[Install]

WantedBy=multi-user.target

Step 4 — Reload systemd and start the service:

```bash
sudo systemctl daemon-reload
sudo systemctl start procored
sudo systemctl enable procored
sudo systemctl status procored
```
## Troubleshooting & Root Cause Analysis

The main issue was that /nfs/incoming/scripts/procored did not exist on one host, making clear this was a filesystem availability or mount issue rather than a permissions problem.

Workaround: either mount the NFS share correctly first, or copy procored from another host where it was already present (scp between VMs).

```bash
systemctl daemon-reload (not daemon-reexec) is the correct command to pick up new unit files. daemon-reexec reloads the systemd manager itself and is rarely needed.
```
## Validation & Testing

```bash
sudo systemctl status procored
ps aux | grep procored
```
## Key Lessons Learned

- systemctl daemon-reload is required after placing a new .service file in /etc/systemd/system/

- If NFS is unavailable, scp from another host is a valid fallback to get the binary

- Use Restart=on-failure in the [Service] section to auto-restart the daemon if it crashes

## Screenshots

***Figure 1: dev-app — procored binary deployment and systemd unit file creation***

***Figure 2: systemctl status procored showing service active and running***
