# Linux Scripting & Task Scheduling — Procore-Plus Lab

Bash scripting, cron scheduling, systemd service creation, and filesystem management on the CLI.

## Environment

CentOS Stream / RHEL-based VMs (`dev-app`, `stage-web`, `dev-performance` hosts) managed as a production-style environment with Jira ticket tracking, monitoring, and change control.

## Skills & Tools

Bash scripting · cron/crontab · systemd unit files · symlinks · system information gathering · performance scripts

## Tickets

| Ticket | Title | Documentation |
| --- | --- | --- |
| #45 | Configure Cron Job for logs.sh (Every 6 Hours) | [view](tickets/ticket-45-cron-logs-script.md) |
| #53 | Create a Symlink | [view](tickets/ticket-53-create-symlink.md) |
| #57 | Create a Script to Gather System Information | [view](tickets/ticket-57-system-info-script.md) |
| #62 | Create a Systemd Service for procored | [view](tickets/ticket-62-systemd-procored-service.md) |
| #65 | Run Performance Script | [view](tickets/ticket-65-performance-script.md) |

## Highlights

- **Service-ify a script (#62):** Turned a standalone binary distributed via NFS into a proper systemd-managed service across three servers, with enable/start verified on each.
- **Scheduled automation (#45):** Set up a recurring cron job to run a log-processing script every 6 hours, with verification that it fired on schedule.
- **Custom tooling (#57):** Wrote a bash script to gather and report system information on demand.

## About

Each ticket document includes the objective, environment details, step-by-step commands, troubleshooting/root-cause notes, and verification of the outcome.
