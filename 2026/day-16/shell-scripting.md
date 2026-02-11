# Shell Scripting Basics

## Task 1: Your First Script

1. Create a file hello.sh
2. Add the shebang line #!/bin/bash at the top
3. Print Hello, DevOps! using echo
4. Make it executable and run it

<img width="501" height="224" alt="image" src="https://github.com/user-attachments/assets/92898d4b-1bb0-4ae1-b335-40c52144d269" />

What happens if you remove the shebang line?

Removing the shebang line in a Linux script means the operating system will not know which interpreter to use automatically. 
Instead of the specified interpreter (like /bin/bash), the script will be executed using the default system shell- typically /bin/sh

<hr/>

## Task 2: Variables 

Create variables.sh with:
   - A variable for your NAME
   - A variable for your ROLE (e.g., "DevOps Engineer")
   - Print: Hello, I am <NAME> and I am a <ROLE>

Try using single quotes vs double quotes — what's the difference?

<img width="463" height="153" alt="image" src="https://github.com/user-attachments/assets/73817f15-0549-4808-9421-19b65dc860d1" />

<img width="454" height="94" alt="image" src="https://github.com/user-attachments/assets/0cd810c7-91f3-4ff5-9e03-73412e9646f4" />

<hr/>

## Task 3: User Input with read

Create greet.sh that:
  - Asks the user for their name using read
  - Asks for their favourite tool
  - Prints: Hello <name>, your favourite tool is <tool>

<img width="512" height="112" alt="image" src="https://github.com/user-attachments/assets/fdcdfb48-78c3-4c24-8f8a-65e3f966e3c9" />

<img width="471" height="118" alt="image" src="https://github.com/user-attachments/assets/f48c2097-fc94-453f-b1bc-4d200f59bad8" />

<hr/>

## Task 4: If-Else Conditions

Create check_number.sh that:

1. Takes a number using read
  - Prints whether it is positive, negative, or zero
  - Create file_check.sh that:

<img width="467" height="230" alt="image" src="https://github.com/user-attachments/assets/7e0d4751-e9af-4266-b98c-2c8e714dd56f" />
<img width="507" height="189" alt="image" src="https://github.com/user-attachments/assets/68a349b6-823e-4206-b744-b85277e6ba26" />

2. Asks for a filename
  - Checks if the file exists using -f
  - Prints appropriate message

<img width="411" height="171" alt="image" src="https://github.com/user-attachments/assets/ef4f6dc7-cd38-41a5-912d-428930762018" />
<img width="586" height="444" alt="image" src="https://github.com/user-attachments/assets/61d87c08-3fae-49a5-85fa-2db964efdc59" />

<hr/>

## Task 5: Combine It All

Create server_check.sh that:

  - Stores a service name in a variable (e.g., nginx, sshd)
  - Asks the user: "Do you want to check the status? (y/n)"
  - If y — runs systemctl status <service> and prints whether it's active or not
  - If n — prints "Skipped."


<img width="522" height="326" alt="image" src="https://github.com/user-attachments/assets/b6af222b-32fc-4447-b403-4fae196511af" />

<img width="974" height="514" alt="image" src="https://github.com/user-attachments/assets/5e37455c-9380-4813-a79a-4e09a7150245" />


