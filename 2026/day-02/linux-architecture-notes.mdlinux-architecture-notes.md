**Core Components**
•	Linux Kernel: The central, low-level core that interacts directly with hardware (CPU, memory, devices). 
It handles process management, memory management, and device drivers.
•	System Libraries: Special functions that allow applications or utilities to request 
services from the kernel without needing direct access to kernel code.
•	System Utilities: Software programs that perform individual, specialized tasks, such as managing files, editing text, or navigating the system (e.g., ls, cd, grep).
•	Shell: A command-line interpreter that acts as an interface between the user and the kernel. Examples include bash, zsh, and sh.
•	Hardware Layer: The physical components of the machine, including the processor (CPU), memory (RAM), and storage devices, which the kernel controls.
•	Bootloader: Software that manages the boot process, such as GRUB, which loads the kernels into memory.
•	Init System: The first process (PID 1) started by the kernel, responsible for initiating user space and managing services (e.g., systemd). 
