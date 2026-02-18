# Bash Scripting Challenge: Log Analyzer and Report Generator

## Task 1: Input and Validation

  - Accept the path to a log file as a command-line argument
  - Exit with a clear error message if no argument is provided
  - Exit with a clear error message if the file doesn't exist

<img width="890" height="559" alt="image" src="https://github.com/user-attachments/assets/146a4f06-8788-481d-8061-3e10e19223d1" />
<img width="881" height="349" alt="image" src="https://github.com/user-attachments/assets/ce5d4c5a-c542-40ed-a83d-5b10e1c42978" />


```
function usage {
        echo "Usage: ./log_analyzer.sh <path of file name>"
        exit 1
}
```


```
LOG_FILE=$1
function check {
        if [ -f $LOG_FILE ]; then
                :
        else
                echo "File doesn't exist."
        fi
}
```

<hr/>

## Task 2: Error Count

   - Count the total number of lines containing the keyword ERROR or Failed
   - Print the total error count to the console

```
# 2. Total Errors
error_count=$(grep -ciE "ERROR|Failed" "$LOG_FILE")
echo "Total errors found: $error_count"
```

<hr/>

## Task 3: Critical Events

   - Search for lines containing the keyword CRITICAL
   - Print those lines along with their line number

```
# 4. Critical events
echo "Critical events list:"
grep -n "CRITICAL" $LOG_FILE
```

<hr/>

## Task 4: Top Error Messages

  - Extract all lines containing ERROR
  - Identify the top 5 most common error messages
  - Display them with their occurrence count, sorted in descending order

```
# 3. Top 5 common errors
echo "Top 5 error messages:"
grep -iE "ERROR|Failed" "$LOG_FILE" | awk '{print $NF}' | sort | uniq -c | sort -nr | head -n 5
```

<hr/>

## Task 5: Summary Report

Generate a summary report to a text file named log_report_<date>.txt (e.g., log_report_2026-02-11.txt). The report should include:

  - Date of analysis
  - Log file name
  - Total lines processed
  - Total error count
  - Top 5 error messages with their occurrence count
  - List of critical events with line numbers

```
function report {
        report="log_report_$(date +%Y-%m-%d-%H-%M).txt"
        echo "File: ~/$report"
        echo "Date Of Analysis : $(date +%Y-%m-%d" Time : "%H:%M)" >> $report
        echo "Name Of Log File : $LOG_FILE" >> $report
        echo "Total lines processed: $total_lines" >> $report
        echo "error_count=$(grep -ciE "ERROR|Failed" "$LOG_FILE")" >> $report
        #echo "grep -iE "ERROR|Failed" "$LOG_FILE" | awk '{print $NF}' | sort | uniq -c | sort -nr | head -n 5" >> $report
        echo "grep -n "CRITICAL" $LOG_FILE" >> $report
}
```
<img width="626" height="195" alt="image" src="https://github.com/user-attachments/assets/5ae03759-f33b-4ace-ac7f-83f6e3785e8f" />

<hr/>

## Task 6 (Optional): Archive Processed Logs

  - Create an archive/ directory if it doesn't exist
  - Move the processed log file into archive/ after analysis
  - Print a confirmation message

<img width="735" height="394" alt="image" src="https://github.com/user-attachments/assets/ad10fadb-ce76-4d98-be5e-e26a789b2ff8" />

```
function move {
        mkdir -p archive
        mv $report archive
        echo -e "\nCreated report file $report and moved it to archive folder."
}
```

<hr/><hr/>

Hints
1. Count errors: grep -c "ERROR" logfile.log
2. Print with line numbers: grep -n "CRITICAL" logfile.log
3. Top occurrences: grep "ERROR" logfile.log | awk '{$1=$2=$3=""; print}' | sort | uniq -c | sort -rn | head -5
4. Associative arrays: declare -A error_map
5. ate for filename: date +%Y-%m-%d
6. Move files: mv logfile.log archive/
