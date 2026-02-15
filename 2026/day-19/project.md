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

