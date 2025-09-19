# experiment 1 : check_file.sh

## in the code mentioned below, bash script checks if a given file exists and offers to create it if it does not. 

![alt text](<images for unit7/Screenshot from 2025-09-19 12-12-08.png>)

### 1. Shebang:-


![alt text](<images for unit7/Screenshot from 2025-09-19 23-20-02.png>)
```
This is the shebang line that tells the system to use the Bash shell to interpret the script.
These are comments describing the script name and how to use it. Comments start with # and are ignored by the shell.
```

### 2.Argument count check:-

![alt text](<images for unit7/Screenshot from 2025-09-19 23-27-49.png>)
```
This checks if the number of arguments passed to the script is exactly 1.
If not, it prints a usage message with the script's own name ($0) and exits with a status of 1, indicating an error.
This ensures the script gets exactly one argument, which should be the filename.
```

### 3. File existence check:-

![alt text](<images for unit7/Screenshot from 2025-09-19 23-31-06.png>)
```
The first argument is assigned to the variable file.
The script checks if a file named in $file exists using the -e test.
If the file exists, it prints that fact along with a separator.
Then it outputs the file contents using cat.
The -- in cat -- protects against filenames starting with a dash.
```

### 4.File does not exist handling:-

![alt text](<images for unit7/Screenshot from 2025-09-19 23-33-56.png>)
```
If the file does not exist, the script notifies the user.
It prompts the user to ask if they want to create the file now, reading their input into ans.
Using a case statement, it checks the user's response.
If the response begins with y or Y, it creates the file with touch and informs the user.
For any other response, it simply states that it won't create the file.
```

## The line by line explation of the code is :-
```
#!/bin/bash
→ Shebang line, tells the system to use bash to run this script.

2–3. Comments explaining the script name and usage.

4–7.

if [ $# -ne 1 ]; then
  echo "Usage: $0 <filename>"
  exit 1
fi


$# = number of command-line arguments.

If not equal to 1, print usage and exit.

$0 is the script name itself.

👉 Ensures the user must provide one filename.

file="$1"

$1 is the first argument passed (the filename).

Stores it in variable file.

10–14.

if [ -e "$file" ]; then
  echo "File exists: $file"
  echo "------ contents ------"
  cat -- "$file"


-e "$file" checks if the file exists.

If yes → print confirmation and display its contents using cat.

15–21 (else part):
If the file does not exist:

Inform the user: "File '$file' does not exist."

Ask: "Create it now? (y/N): "

Use a case statement:

If answer starts with y or Y → touch creates an empty file, then prints confirmation.

Otherwise → "Not creating file."

✅ What this script does

Takes a filename as input.

If file exists → shows its contents.

If file does not exist → asks user whether to create it.

👉 Example run:

$ ./check_file.sh notes.txt
File 'notes.txt' does not exist.
Create it now? (y/N): y
Created notes.txt
You can edit it using nano/vi/etc. 

```