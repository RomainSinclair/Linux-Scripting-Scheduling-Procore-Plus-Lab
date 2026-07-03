# Ticket #53 — Create a Symlink

*Category: Terminal / CLI / Scripting — File Management*

| **Field** | Value |
| --- | --- |
| **Ticket #** | 53 |
| **Title** | Create a Symlink |
| **Category** | Terminal / CLI / Scripting — File Management |
| **Prepared by** | Romain Sinclair |
| **Environment** | Procore-Plus Lab (CentOS Stream / RHEL-based) |

## Objective

Create a symbolic link in the home directory on stage-web pointing to /var/www/html/ariclaw/elements.html, named 'elements'.

## Requirements

- VM: stage-web

- Symlink: ~/elements -> /var/www/html/ariclaw/elements.html

## Environment Details

- VM: stage-web

- Source: /var/www/html/ariclaw/elements.html

- Link: ~/elements

## Implementation Steps

Step 1 — Verify the source file exists:

```bash
ls -l /var/www/html/ariclaw/elements.html
```
Step 2 — Create the symlink in the home directory:

```bash
ln -s /var/www/html/ariclaw/elements.html ~/elements
```
Step 3 — Confirm the symlink was created correctly:

```bash
ls -l ~/elements
```
## Troubleshooting & Root Cause Analysis

The biggest issue was confirming the source path existed and undoing earlier incorrect symlink attempts. If a symlink with the same name already exists, ln -s will fail with 'File exists'.

To fix: remove the incorrect symlink first with rm ~/elements, then re-create it pointing to the correct source.

readlink -f ~/elements verifies where the symlink ultimately resolves to, confirming correctness.

## Validation & Testing

```bash
ls -l ~/elements
```
readlink -f ~/elements

## Key Lessons Learned

- ln -s <source> <link> — source comes first, link name comes second

- If the symlink already exists, remove it with rm before re-creating

- readlink -f resolves the full canonical path of the symlink target

## Screenshots

***Figure 1: stage-web — symlink creation and ls -l confirmation showing elements -**>** elements.html***
