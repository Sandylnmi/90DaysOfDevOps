# Shell Scripting Cheat Sheet: Build Your Own Reference Guide

## Task 1: Basics

Document the following with short descriptions and examples:

* Shebang (#!/bin/bash) — what it does and why it matters. 
`
Shebang is an interpreter directive that tells the operating system to use the Bash shell to execute the commands in the script. It matters because it ensures the script runs with the correct and intended interpreter, making it predictable and portable across different systems.
`
  
* Running a script — chmod +x, ./script.sh, bash script.sh
  
  `chmod+x - Sets the execute permission on a file/script.`
  
  `./script.sh - Run the script by using the interpreter defined in script.`
  
  `bash script.sh - Run the script by explicitly invoking bash interprete.`
  
* Comments — single line (#) and inline
  
  `#this is comment`
  
  `echo "hi"; #this is inline comment`
  
* Variables — declaring, using, and quoting ($VAR, "$VAR", '$VAR')

  `name="sandy" #declaring `
  `echo "$name" #using`
  `echo '$name' #single quote (Output: $name)`
  
* Reading user input — read
  
  `read -p "Enter Name : " name #Take name as an input in runtime and save it in variable 'name'`
  
* Command-line arguments — $0, $1, $#, $@, $?

  `$0 - Filename # check.sh`
  
  `$1 - 1st argument passed by user # check.sh 7`
  
  `$# - Total number of arguments passed # 5`
  
  `$@ - Prints all arguments`
  
  `$? - Last command's exit status`

<hr/>

## Task 2: Operators and Conditionals

Document with examples:

* String comparisons — =, !=, -z, -n

<img width="335" height="529" alt="image" src="https://github.com/user-attachments/assets/cb8837fa-04d5-4cba-822a-f9354ba02420" />
<img width="332" height="72" alt="image" src="https://github.com/user-attachments/assets/36ac8587-aba3-4195-b935-9754be34e61b" />


* Integer comparisons — -eq, -ne, -lt, -gt, -le, -ge

<img width="413" height="663" alt="image" src="https://github.com/user-attachments/assets/dc9f1831-8699-49fd-be1f-7e99653d81f4" />
<img width="313" height="132" alt="image" src="https://github.com/user-attachments/assets/dbf03f3f-942c-4a09-b5ad-44975aff962b" />

  
* File test operators — -f, -d, -e, -r, -w, -x, -s

<img width="425" height="324" alt="image" src="https://github.com/user-attachments/assets/d24a5617-6929-4608-9034-9a52f7f00250" />
<img width="313" height="252" alt="image" src="https://github.com/user-attachments/assets/f54225d2-fe06-4969-9952-9beaef06d6a8" />


* if, elif, else syntax

  <img width="206" height="165" alt="image" src="https://github.com/user-attachments/assets/2a3a8003-dd3f-45a9-b9d8-a17dac45ae69" />

* Logical operators — &&, ||, !

  <img width="361" height="228" alt="image" src="https://github.com/user-attachments/assets/78883383-3e1b-4805-8386-2447318ec7bc" />

* Case statements — case ... esac

  <img width="332" height="264" alt="image" src="https://github.com/user-attachments/assets/a80bc9d8-9e5b-4779-82d7-768418a0d1da" />
  
<hr/>

## Task 3: Loops  

Document with examples:

* for loop — list-based and C-style
* while loop
* until loop
* Loop control — break, continue
* Looping over files — for file in *.log
* Looping over command output — while read line

  <img width="540" height="669" alt="image" src="https://github.com/user-attachments/assets/67a93485-650f-4c71-847b-fd71594b134f" />

<hr/>

## Task 4: Functions

Document with examples:

* Defining a function — function_name() { ... }

`A function must be defined before it is called in a script. There are two common syntaxes.`
  ```Preferred syntax:
  function_name () {
  # commands
  }

  Alternative syntax (using function keyword):
  function function_name {
  # commands
  }
  ```

* Calling a function

`To execute a function, simply use its name`
`function_name`

* Passing arguments to functions — $1, $2 inside functions

`Arguments are passed to a function by listing them after the function's name when it is called. Inside the function, these arguments are accessed using positional parameters.`

`$1, $2, ...: Represent the first, second, and subsequent arguments.`

`$#: Holds the number of arguments passed to the function.`

`$@ or $*: Hold all arguments passed to the function.`

* Return values — return vs echo

`Bash functions primarily use return to set an exit status (a number between 0 and 255) and use echo to output data to standard output.` 

`return:`
`Signals the exit status (success or failure) of the function.`
`The value can be captured by the special variable $? immediately after the function call.`
`Cannot return arbitrary data like strings; it is strictly a numeric status code.`


```check_status() {
return 10
}
check_status
echo "The exit status was: $?" # Output: The exit status was: 10
```

`echo:`
`Sends data (strings, numbers, etc.) to standard output.`
`This output is how you "return" data that you want to use in variable assignments.`
`The output is captured using command substitution $(function_name).`

```get_message() {
  echo "Some result"
}
result=$(get_message)
echo "The result is: $result" # Output: The result is: Some result
```


* Local variables — local

```By default, any variable declared within a Bash function is a global variable (accessible outside the function). 
To create a variable whose scope is limited to within the function, use the local keyword before its first declaration.
It is considered a best practice to use local for variables inside functions to prevent unintended side effects in other parts of the script.

var1="global_value"

my_function() {
  local var1="local_value" # This is a local variable
  var2="changed_global_value" # This modifies the global var2
  echo "Inside function: var1 is $var1, var2 is $var2"
}

echo "Before function call: var1 is $var1, var2 is $var2"
my_function
echo "After function call: var1 is $var1, var2 is $var2"

# Output:
# Before function call: var1 is global_value, var2 is global_value
# Inside function: var1 is local_value, var2 is changed_global_value
# After function call: var1 is global_value, var2 is changed_global_value
```

<hr/>

## Task 5: Text Processing Commands

1. grep — search patterns, -i, -r, -c, -n, -v, -E
* `-i` : Case insensitive
* `-r` : Recursive
* `-c` : Count matching lines
* `-n` : Print each line with line number
* `-v` : Inverted match (Exclude pattern) 
* `-E` : Extended regex
  
2. awk — print columns, field separator, patterns, BEGIN/END
* Print Columns : `awk '{print $1,$2}'`
* Field seperator : `awk -F: '{print $1,$7}'`
* Pattern : `awk '/error/ {print $0}'`
* BEGIN : `awk 'BEGIN {print "START"}{print $3}'`
* END : `awk 'END {print "DONE"}{print $3}'`

3. sed — substitution, delete lines, in-place edit
* substitution : `sed 's/WARNING/CRITICAL/g' file.log` (It only prints the output unless you redirect it to a file. 
    It doesn't change the actual file.)
* delete lines : `sed '2,4d' file.log`
* in-place edit : `sed -i 's/CRITICAL/WARNING/g' file.log` (Edits the file and apply the changes.)

4. cut — extract columns by delimiter
* `cut -d: -f1 /etc/passwd`

5. sort — alphabetical, numerical, reverse, unique
* alphabetical : `sort file.txt`
* numerical : `sort -n file.txt`
* reverse : `sort -r file.txt`
* unique : `sort -u file,txt`

6. uniq — deduplicate, count
* sort sort.txt | uniq -dc

7. tr — translate/delete characters
* translate : `tr [a-z] [A-Z] < sort.txt`
* delete : `tr -d a < sort.txt`

8. wc — line/word/char count
* `wc -wcl sort.txt`

9. head / tail — first/last N lines, follow mode
* head : `head -5 sort.txt`
* tail : `tail -5 sort.txt`

<hr/>

## Task 6: Useful Patterns and One-Liners

* Find and delete files older than N days : `find . -mtime +4`
* Count lines in all `.log` files :`wc -l file.log`
* Replace a string across multiple files : `sed 's/hello/bye/g' *.txt`
* Check if a service is running : `systemctl status ssh`
* Monitor disk usage with alerts : `df -h | awk '$5>80'`
* Parse CSV or JSON from command line : `cat data.json | jq '.' or `jq '.' data.json`
* Tail a log and filter for errors in real time : `tail -f file.log | grep -i "error"`

<hr/>

## Task 7: Error Handling and Debugging

* Exit codes — $?, exit 0, exit 1
  - $? - Exit status of last command (0/1)
  - exit 0 - Exit if success
  - exit 1 - Exit if error
* set -e — exit on error
* set -u — treat unset variables as error
* set -o pipefail — catch errors in pipes
* set -x — debug mode (trace execution)

```set -euo pipefail
set -x

echo "Check set -o pipefail"
cat count.txt | grep "total"
echo "After failing script running without set -o"

echo -e "\n"
echo "Undefined variable -u"
echo $a
echo "After using undefined variable script running without set -u"

echo -e "\n"
echo "Failed command -e"
mkdir ../scripts
echo "After failing command script running without using -e"
```

* Trap — trap 'cleanup' EXIT - `It means on exit CleanUp will be printed. You can even define a function cleanup() and write any code you want inside ex: rm /archive/*.gz. cleanup function will be executed on EXIT of a script.`

<hr/><hr/>

## Reference Table

| Topic | Key Syntax | Example |
|-------|-----------|---------|
| Variable | `VAR="value"` | `NAME="DevOps"` |
| Argument | `$1`, `$2` | `./script.sh arg1` |
| If | `if [ condition ]; then` | `if [ -f file ]; then` |
| For loop | `for i in list; do` | `for i in 1 2 3; do` |
| Until | `until [ condition ] do .. done` | `until [ $i -eq 5 ] do echo $i ((i++)) done` |
| Function | `name() { ... }` | `greet() { echo "Hi"; }` |
| Grep | `grep pattern file` | `grep -i "error" log.txt` |
| Awk | `awk '{print $1}' file` | `awk -F: '{print $1}' /etc/passwd` |
| Sed | `sed 's/old/new/g' file` | `sed -i 's/foo/bar/g' config.txt` |
| Case | `case variable in a);; b);; *);; esac` | `case $num in a);; b);; *);; esac` |
| Sort | `sort file` | `sort -n log.txt` |
| Tr | `tr option [set1] [set2] < file` | `tr [a-z] [A-Z] < file` |
| Wc | `wc option file` | `wc -wcl log.txt` |
| Head | `head -n file` | `head -10 log.txt` |
| Tail | `tail -n file` | `tail -10 log.txt` |

