# Linux File System Hierarchy
<img width="689" height="327" alt="image" src="https://github.com/user-attachments/assets/b5402024-08ba-44be-ab34-3c22d87220a3" />
<img width="761" height="669" alt="image" src="https://github.com/user-attachments/assets/52248222-5835-49c9-a4b7-0071ca0b13c8" />

- / (Root): The top-level directory; all files and directories start here.
- /bin & /usr/bin: Essential command binaries (e.g., ls, cd, cat).
- /sbin & /usr/sbin: System binaries for administration (e.g., iptables, reboot).
- /etc: System-wide configuration files.
- /home: User home directories (e.g., /home/username).
- /var: Variable data, such as logs (/var/log), mail, and databases.
- /tmp: Temporary files, often cleared on reboot.
- /boot: Bootloader files and Linux kernel.
- /dev: Device files (e.g., /dev/sda for hard drives).
- /lib & /usr/lib: Shared libraries required by binaries in /bin and /sbin.
- /media & /mnt: Mount points for removable media (USB) and filesystems.
- /root: Home directory for the root user.
- /proc & /sys: Virtual filesystems providing kernel and process information.

<img width="518" height="369" alt="image" src="https://github.com/user-attachments/assets/9616a649-f912-48ed-ac2d-2cb58bfa0808" />

<hr/>

## Scenario 1: Service Not Starting
`A service called 'ssh' failed to start after a server reboot. Below commands need to run to diagnose the issue.`

Step 1 : `systemctl status ssh`

Why : Check if the service is running or failed or stopped.

Step 2 : `systemctl is-enabled ssh`

Why : To check if service starts automatically on boot.

Step 3 : `journalctl -u ssh -n 20`

Why : If service is failed check logs.

<img width="843" height="612" alt="image" src="https://github.com/user-attachments/assets/9a998780-506d-4121-ba51-3227ce612ba6" />

## Scenario 2: High CPU Usage

`Manager reports that the application server is slow. We SSH into the server. Below commands need to run to identify which process is using high CPU.`

Step 1 : `top/htop`

Why : List all the running processes. Check for processes that are CPU intensive.

Step 2 : `ps -aux --sort=-%cpu | head -10`

Why : Sort the processes by CPU percentage. Note down PID of top processes.

Step 3 : `sudo renice +10 -p <PID>`

Why : Increases the nice value, lowering the process priority so it gets less CPU time. Useful if you don't want to kill the process but need to reduce its impact.

Step 4 : `kill PID`

Why : Kill CPU intensive processes if necessary.

## Scenario 3: Finding Service Logs
`Developer asks: "Where are the logs for the 'docker' service?" The service is managed by systemd.`

Step 1 : `systemctl status ssh`

Why : Check service status first.

Step 2 : `journalctl -u ssh -n 20`

Why : Check last 20 lines of logs.

Step 3 : `journalctl -u ssh -f`

Why : Check logs real-time.

## Scenario 4: File Permissions Issue
`A script at /home/user/backup.sh is not executing. When you run it: ./backup.sh. get: "Permission denied"`

Step 1 : `ls -l /home/user/backup.sh`

Why : Check current permissions of file. Look for: -rw-r--r-- (notice no 'x' = not executable).

Step 2 : `chmod +x /home/user/backup.sh`

Why : Add execute permission to file.

Step 3: `ls -l /home/user/backup.sh`

Why : Verify it worked. Look for: -rwxr-xr-x (notice 'x' = executable).

Step 4: `./backup.sh`

Why : Run it.
