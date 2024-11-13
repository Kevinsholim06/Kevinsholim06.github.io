<!-- ---
title: "Obsidian backup and sync"
date: 2024-09-24T06:00:23+06:00
hero: /images/posts/writing-posts/analytics.svg
description: Obsidian backup and sync in linux 
theme: Toha
menu:
  sidebar:
    name: Obsidian backup and sync
    identifier: Obsidian_backup_and_sync
    weight: 30

---

Automating Backups and Syncs for Your Obsidian Vault

In this guide, we’ll explore how to automate the backup and synchronization of your Obsidian Vault using shell scripts and systemd service files. This setup ensures that your notes are safely backed up before shutdown and synced with GitHub whenever your system connects to the network.

---

## Backup Process

### Step 1: Create the Backup Script

First, we need a shell script that performs the backup. Create a file named `obsidian_backup.sh` in your home directory:
```sh
#!/bin/bash

# Variables
REPO_PATH="/home/sholim/Documents/'Obsidian Vault'"
LOG_FILE="/home/sholim/Documents/'Obsidian Vault'/backup.log"
DATE=$(date +"%Y-%m-%d %H:%M:%S")

# Navigate to repo
cd $REPO_PATH

# Add and commit changes
git add .
git commit -m "Backup on $DATE"

# Push changes to GitHub
git push origin main

# Log the result
if [ $? -eq 0 ]; then
    echo "$DATE: Backup successful" >> $LOG_FILE
else
    echo "$DATE: Backup failed" >> $LOG_FILE
fi
```
### Step 2: Create the Systemd Service File

Next, we’ll set up a systemd service to run our backup script. Create a new service file named `obsidian-backup.service`:
``sudo nano /etc/systemd/system/obsidian-backup.service``
Add the following content to the file:
```ini
[Unit]
Description=Obsidian Backup Service
DefaultDependencies=no
Before=shutdown.target

[Service]
Type=oneshot
ExecStart=/bin/bash /home/sholim/obsidian_backup.sh
User=sholim

[Install]
WantedBy=halt.target reboot.target shutdown.target
```

### Step 3: Enable the Backup Service

Finally, enable the service so it runs at shutdown:
```sh
sudo systemctl daemon-reload
sudo systemctl enable obsidian-backup.service
```

## Sync Process

### Step 1: Create the Sync Script

Next, let’s create a script to sync your changes with GitHub. Create a file named `obsidian_sync.sh` in your home directory:
```
#!/bin/bash

# Variables
REPO_PATH="/home/sholim/Documents/'Obsidian Vault'"
LOG_FILE="/home/sholim/Documents/'Obsidian Vault'/sync.log"
DATE=$(date +"%Y-%m-%d %H:%M:%S")

# Navigate to repo
cd $REPO_PATH

# Pull latest changes from GitHub
git pull origin main

# Add and commit changes
git add .
git commit -m "Sync on $DATE"

# Push changes to GitHub
git push origin main

# Log the result
if [ $? -eq 0 ]; then
    echo "$DATE: Sync successful" >> $LOG_FILE
else
    echo "$DATE: Sync failed" >> $LOG_FILE
fi
```

### Step 2: Create the Systemd Service File for Syncing

Now, we’ll set up a systemd service to run our sync script. Create a new service file named `obsidian-sync.service`:
``sudo nano /etc/systemd/system/obsidian-sync.service

Add the following content to the file:
```sh
```sh
[Unit]
Description=Obsidian Sync Service
After=network-online.target

[Service]
Type=oneshot
ExecStart=/bin/bash /home/sholim/obsidian_sync.sh
User=sholim

[Install]
WantedBy=default.target
```

### Step 3: Enable the Sync Service

Enable the sync service so it runs automatically after startup when the network is available:
```sh
sudo systemctl daemon-reload sudo systemctl enable obsidian-sync.service
```

## Conclusion

By following these steps, you've set up automated backups and synchronization for your Obsidian Vault. Your notes will now be backed up before shutdown and synced with GitHub every time your machine starts and connects to the network. This setup not only saves time but also ensures your valuable notes are secure. Happy writing! -->