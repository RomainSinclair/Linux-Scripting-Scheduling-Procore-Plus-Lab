# Ticket #45 — Configure Cronjob for logs.sh Script

*Category: Terminal / CLI / Scripting — Cron*

| **Field** | Value |
| --- | --- |
| **Ticket #** | 45 |
| **Title** | Configure Cronjob for logs.sh Script |
| **Category** | Terminal / CLI / Scripting — Cron |
| **Prepared by** | Romain Sinclair |
| **Environment** | Procore-Plus Lab (CentOS Stream / RHEL-based) |

## Objective

Configure a cron job on dev-app to run the logs.sh script every 6 hours. The script must be copied locally, permissions verified, and the cron job confirmed with output logs written to /lfjs.

## Requirements

- VM: dev-app

- Script: /nfs/incoming/scripts/logs.sh

- Cron job runs every 6 hours

- Copy the script locally to /usr/local/bin/logs.sh

- Verify script permissions and cron job configuration

- Verify logs are generated in /lfjs

## Environment Details

- VM: dev-app

- Script: /usr/local/bin/logs.sh (deployed locally)

- Cron log: /var/log/logs_sh_cron.log

- Output path: /lfjs, /lfjs/logs

## Implementation Steps

Step 1 — Copy the script from NFS to the local path:

```bash
sudo cp /nfs/incoming/scripts/logs.sh /usr/local/bin/logs.sh
```
Step 2 — Set ownership and permissions:

```bash
sudo chown root:root /usr/local/bin/logs.sh
sudo chmod 755 /usr/local/bin/logs.sh
```
Step 3 — Preview the script content:

head -n 5 /usr/local/bin/logs.sh

Step 4 — Edit root's crontab and add the job:

```bash
sudo crontab -e
```
Add: 0 */6 * * * /usr/local/bin/logs.sh >> /var/log/logs_sh_cron.log 2>&1

Step 5 — Verify the cron entry:

```bash
sudo crontab -l
```
Step 6 — Confirm crond is running:

```bash
sudo systemctl status crond --no-pager
```
Step 7 — Run the script manually to test immediately:

```bash
sudo /usr/local/bin/logs.sh
```
Step 8 — Check outputs:

```bash
sudo tail -n 50 /var/log/logs_sh_cron.log
sudo ls -lah /lfjs
sudo find /lfjs -type f -mmin -10 -ls
```
## Troubleshooting & Root Cause Analysis

The NFS path initially did not contain the script as expected, causing confusion during the copy step. There was also a filename case confusion: Logs.sh (capital L) versus logs.sh (lowercase).

Resolution: locate the actual script at its correct path (it was already present at /usr/local/bin/logs.sh from a previous session), standardize the filename to lowercase, assign executable permissions, and confirm crond service state.

Validation then focused on /lfjs output and cron logs to confirm the script ran and produced the expected files.

## Validation & Testing

```bash
sudo crontab -l
sudo systemctl status crond --no-pager
sudo tail -n 50 /var/log/logs_sh_cron.log
sudo find /lfjs -type f -mmin -10 -ls
```
## Key Lessons Learned

- 0 */6 * * * means 'at minute 0 of every 6th hour' — correct syntax for every 6 hours

- Always redirect cron output (>> logfile 2>&1) to capture both stdout and stderr for troubleshooting

- Run the script manually first before waiting for cron to fire — confirms the script works independently

