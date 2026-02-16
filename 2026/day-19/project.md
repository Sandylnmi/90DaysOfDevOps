# Shell Scripting Project: Log Rotation, Backup & Crontab

## Task 1: Log Rotation Script

Create log_rotate.sh that:

  - Takes a log directory as an argument (e.g., /var/log/myapp)
  - Compresses .log files older than 7 days using gzip
  - Deletes .gz files older than 30 days
  - Prints how many files were compressed and deleted
  - Exits with an error if the directory doesn't exist

<img width="687" height="678" alt="image" src="https://github.com/user-attachments/assets/4cb5a789-f3fb-4e2c-9e77-f5d8ef8bed2f" />
<img width="623" height="252" alt="image" src="https://github.com/user-attachments/assets/1ce71d7d-1522-409c-a27e-9dedf3205a1b" />

<hr/>

## Task 2: Server Backup Script

Create backup.sh that:

  - Takes a source directory and backup destination as arguments
  - Creates a timestamped .tar.gz archive (e.g., backup-2026-02-08.tar.gz)
  - Verifies the archive was created successfully
  - Prints archive name and size
  - Deletes backups older than 14 days from the destination
  - Handles errors — exit if source doesn't exist

<img width="931" height="535" alt="image" src="https://github.com/user-attachments/assets/9b9058c2-f379-4478-b9ea-7f394a769a81" />
<img width="945" height="319" alt="image" src="https://github.com/user-attachments/assets/4518046b-4ea6-4afb-a36f-b9b10dbf6352" />
<img width="1348" height="331" alt="image" src="https://github.com/user-attachments/assets/340e5864-f2fe-4141-b354-c86375957a8b" />

<hr/>

## Task 3: Crontab

  - Read: crontab -l — what's currently scheduled?
  - Understand cron syntax:
* * * * *  command
│ │ │ │ │
│ │ │ │ └── Day of week (0-7)
│ │ │ └──── Month (1-12)
│ │ └────── Day of month (1-31)
│ └──────── Hour (0-23)
└────────── Minute (0-59)
  - Write cron entries (in your markdown, don't apply if unsure) for:
  - Run log_rotate.sh every day at 2 AM - '0 2 * * *`
  - Run backup.sh every Sunday at 3 AM - `0 3 * * 7`
  - Run a health check script every 5 minutes - `*/5 * * * *`

<hr/>

## Task 4: Combine — Scheduled Maintenance Script
 
  - Calls your log rotation function
  - Calls your backup function
  - Logs all output to /var/log/maintenance.log with timestamps
  - Write the cron entry to run it daily at 1 AM

<img width="945" height="304" alt="image" src="https://github.com/user-attachments/assets/42bbdd37-ce3b-44db-b4a7-81285a1c6909" />

