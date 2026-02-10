# File Permissions & File Operations Challenge
## File Creation & Read Files
<img width="530" height="456" alt="image" src="https://github.com/user-attachments/assets/87f20b32-816d-4427-ac90-2326d0d30514" />

## File Permissions 

`-rw-rw-r--` or `drw-rw-r--`
* `-` → indicates it’s a regular file (not a directory or special file).
* `rw-` → (user/owner) → read + write, no execute.
* `rw-` → (group) → read + write, no execute.
* `r--` → (others) → read only, no write or execute.
* `d` → indicates directory

<img width="472" height="156" alt="image" src="https://github.com/user-attachments/assets/01e36dd5-7be9-44db-b03b-1ce63de0b5ea" />
<img width="346" height="305" alt="image" src="https://github.com/user-attachments/assets/68248101-18d1-4f5a-984e-87095d208633" />

## Modify Permissions 

 * Make script.sh executable → run it with ./script.sh
 * Set devops.txt to read-only (remove write for all)
 * Set notes.txt to 640 (owner: rw, group: r, others: none)
 * Create directory project/ with permissions 755
<img width="502" height="578" alt="image" src="https://github.com/user-attachments/assets/b53c2e23-7da9-4703-a314-94564bda537e" />

 * Try writing to a read-only file - what happens?
 * Try executing a file without execute permission

<img width="491" height="267" alt="image" src="https://github.com/user-attachments/assets/0c6273ec-e900-4b73-9372-30ece97ca394" />

<hr/>

## Commands

* touch fname - Creates empty file.
* echo "Hello" > fname - Create file with content.
* vim fname - Create/open file in Vim.
* cat fname - Prints files content.
* vim -R fname - Open file in read only mode.
* cat /etc/passwd | head -5 - Prints first 5 lines of /etc/passwd.
* cat /etc/passwd | tail -5 - Prints last 5 lines of /etc/passwd.
* chmod +x fname - Adding executable permission for all(owner,group,others).
* chmod -w fname - Removing write permission for all(owner,group,others).
* mkdir -m 755 dname - Create directory with permissions(rwx,r-x,r-x).
