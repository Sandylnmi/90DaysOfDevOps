# Shell Scripting: Loops, Arguments & Error Handling

## Task 1: For Loop

1. Create for_loop.sh that: Loops through a list of 5 fruits and prints each one

<img width="684" height="146" alt="image" src="https://github.com/user-attachments/assets/f03c80e2-8aba-4ad7-a249-18ed46ae0c70" />

<img width="482" height="133" alt="image" src="https://github.com/user-attachments/assets/0eb202b3-96bc-4e53-8cf3-535652e841f5" />

2. Create count.sh that: Prints numbers 1 to 10 using a for loop

<img width="308" height="86" alt="image" src="https://github.com/user-attachments/assets/a8bb4b66-f264-4fe3-8e11-a6cd5e7a0b4d" />
<img width="467" height="198" alt="image" src="https://github.com/user-attachments/assets/b6ba8947-a8e3-4c01-8cfb-fb92e698eca7" />

<hr/>

## Task 2: While Loop

Create countdown.sh that: Takes a number from the user
  Counts down to 0 using a while loop
  Prints "Done!" at the end

`Note - The colon character (:) in a shell script is a null command or no-operation (noop) utility that performs no action but returns an exit status of 0 (success).`  

<img width="387" height="329" alt="image" src="https://github.com/user-attachments/assets/14b1d83b-cfca-4c9d-b4d2-676c1a5012ff" />
<img width="413" height="364" alt="image" src="https://github.com/user-attachments/assets/6c3b83d8-751a-440b-b32f-61de2a3bda62" />

<hr/>

## Task 3: Command-Line Arguments

1. Create greet.sh that:
  - Accepts a name as $1
  - Prints Hello, <name>!
  - If no argument is passed, prints "Usage: ./greet.sh "

`Note - In shell scripts, the special parameter $# is used to store and access the number of command-line arguments (positional parameters) passed to the script or a function within the script. It expands to a decimal number representing the count of arguments provided, not counting the script name itself, which is stored in $0`

<img width="664" height="239" alt="image" src="https://github.com/user-attachments/assets/9eb0462b-6561-4984-ba59-2196826b351b" />
<img width="402" height="131" alt="image" src="https://github.com/user-attachments/assets/671ae848-93ed-452b-a00b-383b02c60236" />

2. Create args_demo.sh that:

  - Prints total number of arguments ($#)
  - Prints all arguments ($@)
  - Prints the script name ($0)

<img width="296" height="91" alt="image" src="https://github.com/user-attachments/assets/d750afd2-5077-4b0c-96dd-4dc278b8678e" />
<img width="401" height="160" alt="image" src="https://github.com/user-attachments/assets/027c1295-fe56-422c-b644-267d0d1c6b76" />

<hr/>

## Task 4: Install Packages via Script

Create install_packages.sh that:
  - Defines a list of packages: nginx, curl, wget
  - Loops through the list
  - Checks if each package is installed (use dpkg -s or rpm -q)
  - Installs it if missing, skips if already present
  - Prints status for each package

<img width="598" height="330" alt="image" src="https://github.com/user-attachments/assets/11f9585b-f656-408c-8336-b3a48e65dcc9" />
<img width="797" height="595" alt="image" src="https://github.com/user-attachments/assets/2361bf7c-ca2e-46b0-9586-f2787508c6e9" />

<hr/>

## Task 5: Error Handling

Create safe_script.sh that:
  - Uses set -e at the top (exit on error)
  - Tries to create a directory /tmp/devops-test
  - Tries to navigate into it
  - Creates a file inside
  - Uses || operator to print an error if any step fails

<img width="1318" height="171" alt="image" src="https://github.com/user-attachments/assets/09a53c39-3d78-49ca-9513-2827d090b0f9" />

<img width="527" height="168" alt="image" src="https://github.com/user-attachments/assets/dffb0bb5-2ad1-4a41-8ee8-518651795a5e" />


Modify your install_packages.sh to check if the script is being run as root — exit with a message if not.

<img width="912" height="429" alt="image" src="https://github.com/user-attachments/assets/83351502-c2d8-46e1-9de2-d7319ea63de4" />

<img width="803" height="707" alt="image" src="https://github.com/user-attachments/assets/20351e58-4772-4980-bc9d-09e66708dea5" />

<hr/>
<hr/>

  - For loop: for item in list; do ... done
  - While loop: while [ condition ]; do ... done
  - Arguments: `$1` first arg, `$#` count, `$@` all args
  - Check root: if [ "$EUID" -ne 0 ]; then echo "Run as root"; exit 1; fi
  - Check package: dpkg -s <pkg> &> /dev/null && echo "installed"
