<h1>Process Management</h1> 

<p><b>ProcessManagement</b> in Linux involves controlling, monitoring, and scheduling running programs (processes) to ensure system stability and efficiency. Key tasks include identifying processes via unique PIDs, utilizing commands like ps (snapshot), top (real-time), kill (terminate), and nice (prioritize), while managing foreground, background, and daemon processes.</p> 
<p>A process is an instance of a running program, encompassing code, data, and resources.</p>
<p>A unique identification number assigned to every process for management purposes.- PID</p>
<p>Processes transition through states: Running/Runnable, Interruptible Sleep, Uninterruptible Sleep, Stopped, and Zombie – Process States</p>
<h4>Types of Processes</h4>
<ul><li><b>Foreground:</b> Interactive processes requiring user input.</li>
<li><b>Background:</b> Non-interactive, autonomous processes.</li>
<li><b>Daemons:</b> System services running in the background, usually started at boot.</li></ul>

<h4>Process Management Commands</h4>
<ul><li><b>ps:</b> Provides a snapshot of current running processes (e.g., ps aux for all processes).</li>
<li><b>top/htop:</b> Offers a real-time, interactive view of resource usage (CPU/memory).</li>
<li><b>kill:</b> Sends signals to processes (default is SIGTERM for termination, -9 for immediate, forceful kill).</li>
<li><b>nice/renice:</b> Adjusts the priority of a process (lower values = higher priority).</li>
<li><b>bg/fg:</b> Resumes suspended jobs in the background or brings them to the foreground.</li>
<li><b>jobs:</b> Lists all active background jobs in the current terminal.</li>
<li><b>&:</b> Appended to a command to run it in the background immediately.</li></ul>

<hr/>
<h1>Linux File System</h1>
<p>The <b>Linux File System</b> is a structured method of storing and organizing data on a Linux machine. It arranges files in a hierarchical directory format starting from the root directory</p>

<p>The <b>Linux file system hierarchy</b> is a tree-like structure based on the Filesystem Hierarchy Standard (FHS), where all files and directories branch out from a single root directory, represented by a forward slash (/).  <b>$tree / -L 1</b></p>

<img width="689" height="327" alt="image" src="https://github.com/user-attachments/assets/552e11aa-4402-4af6-9cf0-6a6c4a5c4bd1" />
<img width="261" height="498" alt="image" src="https://github.com/user-attachments/assets/93825c41-8b19-4361-a2aa-814b6db04b9b" />

<h4>File System Commands</h4>
<ul><li><b>pwd:</b> Print Working Directory; displays the full path of your current location in the filesystem.</li>
<li><b>cd ~:</b> Moves to the home directory.</li>
<li><b>cd ..:</b> Moves up one directory level.</li>
<li><b>mkdir <i>directory_name:</i></b> Creates a new directory.</li>
<li><b>rmdir <i>directory_name:</i></b> Removes an empty directory.</li>
<li><b>touch <i>filename:</i></b> Creates an empty file or updates the access/modification timestamps of an existing file.</li>
<li><b>cp <i>source_file destination:</i></b> Copies files or directories. Use the -r (recursive) option to copy directories.</li>
<li><b>mv <i>old_name new_name</i> or <i>source_file destination:</i></b> Moves or renames files and directories.</li>
<li><b>rm <i>filename:</i></b> Removes a file. Use the -r option to recursively remove directories and their contents.</li>
<li><b>ln:</b> Creates links (shortcuts) to files. Use -s for symbolic (soft) links.</li>
<li><b>cat <i>filename:</i></b> Concatenates files and displays their content on the standard output.</li>
<li><b>head <i>filename:</i></b> Displays the first few lines of a file.</li>
<li><b>tail <i>filename:</i></b> Displays the last few lines of a file.</li>
<li><b>grep <i>"pattern" filename:</i></b> Searches for a specific pattern within files.</li>
<li><b>vim <i>filename:</i></b> Opens a text editor to edit files directly in the terminal.</li>
<li><b>chmod permissions <i>filename:</i></b> Change file permissions (e.g., chmod 755 script.sh).</li>
<li><b>chown user:group <i>filename:</i></b> Change file owner and group.</li> 
<li><b>df:</b> Displays disk space usage for filesystems. The -h option provides human-readable output (e.g., GB, MB).</li>
<li><b>du:</b> Displays the disk usage of files and directories.</li>
<li><b>lsblk:</b> Lists information about all block devices (hard drives, partitions, etc.).</li>
<li><b>mount and umount:</b> Mounts or unmounts a file system to a specific directory.</li>
<li><b>mkfs:</b> Used to format a partition with a specific file system type (e.g., ext4, xfs).</li></ul>
<hr/>
<h1>Networking Troubleshooting Commands</h1>
<ul><li><b>ip:</b> The modern, primary command for managing network interfaces, addresses, and routing.</li>
<li><b>ifconfig:</b> An older command (largely replaced by ip) used to display or configure network interfaces, assign IP addresses, and view MAC addresses.</li>
<li><b>hostname:</b> Displays or sets the system's hostname.</li>
<li><b>ping:</b> Tests host reachability and measures round-trip time of packets using ICMP.</li>
<li><b>traceroute:</b> Displays the path (hops) that packets take to reach a destination, helping to diagnose routing issues and latency.</li>
<li><b>dig (Domain Information Groper):</b> A utility for querying DNS name servers and retrieving information about DNS records (e.g., A, MX, NS records).</li>
<li><b>nslookup:</b> An older, interactive tool for DNS lookups and troubleshooting DNS issues.</li>
<li><b>host:</b> A simple utility for performing DNS lookups to resolve hostnames or IP addresses.</li>
<li><b>ssh (Secure Shell):</b> Establishes a secure, encrypted connection to a remote system for command execution or interactive shell sessions.</li>
<li><b>scp (Secure Copy):</b> Securely copies files between hosts on a network using the SSH protocol.</li>
<li><b>curl/wget:</b> Command-line utilities for transferring data from or to a server using various protocols (HTTP, HTTPS, FTP, etc.).</li></ul>





