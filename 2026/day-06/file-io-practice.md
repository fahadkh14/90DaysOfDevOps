# File I/O Practice

## Objective

Practice basic file creation, writing, appending, and reading in Linux.

### Step 1: Create a File


touch notes.txt


Creates an empty file named `notes.txt`.

### Step 2: Write Text Using Redirection


echo "Learning Linux file operations" > notes.txt
echo "Using redirection operators" >> notes.txt
echo "Practicing DevOps fundamentals" >> notes.txt


Writes and appends text to the file.

### Step 3: Use tee Command


echo "This line was added using tee" | tee -a notes.txt


Displays the line on screen and appends it to the file.

### Step 4: Add More Lines


echo "Line 5: Reading files" >> notes.txt
echo "Line 6: Using cat command" >> notes.txt
echo "Line 7: Using head command" >> notes.txt
echo "Line 8: Using tail command" >> notes.txt


### Step 5: Read Full File


cat notes.txt


Displays the complete file content.

### Step 6: Read First Two Lines


head -n 2 notes.txt


Displays the first two lines of the file.

### Step 7: Read Last Two Lines


tail -n 2 notes.txt


Displays the last two lines of the file.

## Sample File Content


Learning Linux file operations
Using redirection operators
Practicing DevOps fundamentals
This line was added using tee
Line 5: Reading files
Line 6: Using cat command
Line 7: Using head command
Line 8: Using tail command


## Key Learnings

* `touch` creates an empty file.
* `>` overwrites file content.
* `>>` appends content to a file.
* `tee` writes and displays output simultaneously.
* `cat` reads the complete file.
* `head` shows the beginning of a file.
* `tail` shows the end of a file.

