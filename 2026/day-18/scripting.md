# Shell Scripting: Functions & Slightly Advanced Concepts

## Task 1: Basic Functions

Create functions.sh with:
  - A function greet that takes a name as argument and prints Hello, <name>!
  - A function add that takes two numbers and prints their sum
  - Call both functions from the script

<img width="259" height="232" alt="image" src="https://github.com/user-attachments/assets/e3aaf50d-94f2-4728-a56a-d84a493360fb" />
<img width="395" height="134" alt="image" src="https://github.com/user-attachments/assets/19e62f39-77b2-4eeb-8500-2d1773a4d2a6" />

<hr/>

## Task 2: Functions with Return Values

Create disk_check.sh with:
  - A function check_disk that checks disk usage of / using df -h
  - A function check_memory that checks free memory using free -h
  - A main section that calls both and prints the results

<img width="539" height="285" alt="image" src="https://github.com/user-attachments/assets/b804e2f8-62ef-42cc-b38a-3f2afbe8b226" />
<img width="587" height="254" alt="image" src="https://github.com/user-attachments/assets/f5df5461-250e-4c6a-9a1a-b9a16ffb6593" />

<hr/>

## Task 3: Strict Mode — set -euo pipefail

Create strict_demo.sh with set -euo pipefail at the top
  - Try using an undefined variable — what happens with set -u?
  - Try a command that fails — what happens with set -e?
  - Try a piped command where one part fails — what happens with set -o pipefail?

<img width="425" height="210" alt="image" src="https://github.com/user-attachments/assets/43bc406f-ecc1-480b-80b5-a38cdb62d012" />
<img width="526" height="206" alt="image" src="https://github.com/user-attachments/assets/76099703-010d-4b9a-98ae-71580b9563be" />

set -u (nounset)

When the script tries to use $UNDEFINED_VAR, which is not defined:
The script immediately exits with an error like:
`./strict_demo.sh: line 7: UNDEFINED_VAR: unbound variable`

set -e (exit on error)
If we comment out the undefined variable section and run again:
`false`

false returns exit status 1

Because of set -e, the script exits immediately.

Lines after false will NOT execute.

set -o pipefail
If we comment out the first two failing sections and run:
`echo "hello" | grep "nomatch" | wc -l`

grep "nomatch" returns exit code 1

Normally (without pipefail), the pipeline exit status would be from wc -l (which succeeds)

With pipefail, the pipeline fails because grep failed

The script exits immediately

| Option        | Behavior                                 |
| ------------- | ---------------------------------------- |
| `-e`          | Exit immediately if any command fails    |
| `-u`          | Exit if using an undefined variable      |
| `-o pipefail` | Fail pipeline if ANY command in it fails |

<hr/>

## Task 4: Local Variables

Create local_demo.sh with:
  - A function that uses local keyword for variables
  - Show that local variables don't leak outside the function
  - Compare with a function that uses regular variables

<img width="365" height="270" alt="image" src="https://github.com/user-attachments/assets/ec229ecb-970f-4f81-8f34-854fac63c76a" />
<img width="399" height="117" alt="image" src="https://github.com/user-attachments/assets/21867822-27c5-4c74-8936-f973bdc1b8df" />
 

* `Local variables` are confined to the current shell or function in which they are defined.
* `Global variables` (also called environment variables) are accessible throughout the current shell session and any child processes it spawns. 

<hr/>

## Task 5: Build a Script — System Info Reporter

Create system_info.sh that uses functions for everything:

  - A function to print hostname and OS info
  - A function to print uptime
  - A function to print disk usage (top 5 by size)
  - A function to print memory usage
  - A function to print top 5 CPU-consuming processes
  - A main function that calls all of the above with section headers

Use set -euo pipefail at the top

<img width="482" height="704" alt="image" src="https://github.com/user-attachments/assets/6961bb40-2581-4892-98a4-169680ef523b" />

<img width="829" height="701" alt="image" src="https://github.com/user-attachments/assets/c5bd1502-85ec-49d6-b3d7-4dac63437077" />
