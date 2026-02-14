Fundamental Linux File I/O Practice

This hands-on exercise demonstrates core Linux file handling operations using essential commands. It covers file creation, writing, appending, viewing content, and safely writing to privileged locations.

1️⃣ Creating a New File
Commands Executed
touch isaac.txt
ls -l

Result Observed
-rw-r--r-- 1 root root 0 Feb 14 20:21 isaac.txt

Explanation

A new empty file named isaac.txt was created.
The file size is 0 bytes, confirming it contains no data yet.

2️⃣ Writing Initial Content (Overwrite Mode)
Commands Executed
echo "This is line 1" > isaac.txt
cat isaac.txt

Output
This is my personal Info

Explanation

The > redirection operator writes content to the file.

If the file already exists, its previous contents are completely replaced.

This is called overwrite mode.

3️⃣ Adding More Content (Append Mode)
Commands Executed
echo "This is my very very personal Information" >> isaac.txt
cat isaac.txt

Output
This is my personal Info
This my very very personal Information

Explanation

The >> operator appends data to the end of the file.

Existing content is preserved.

New content is added at the bottom.

This is called append mode.

4️⃣ Viewing the Beginning of the File
Command
head -n 2 isaac.txt

Output
This is my personal Info
This my very very personal Information

Explanation

head displays the first lines of a file.

Useful for:

Quickly previewing content

Checking headers

Inspecting large files without opening the entire file

5️⃣ Viewing the End of the File
Command
tail -n 2 isaac.txt

Output
This is my personal Info
This my very very personal Information

Explanation

tail shows the last lines of a file.

Common uses:

Checking recent log entries

Verifying appended data

Monitoring live logs

6️⃣ Writing to a File While Displaying Output (tee)
Objective

Create a sudoers configuration file for user khan.

Commands Executed
echo "root ALL=(ALL) NOPASSWD:ALL" | tee /etc/sudoers.d/root
cat /etc/sudoers.d/root

Output
root ALL=(ALL) NOPASSWD:ALL

Explanation

The tee command sends output to two destinations:

The terminal (so you can see it)

The specified file

Additional notes:

tee -a → Appends to a file

tee (without -a) → Overwrites the file if it exists

This method is especially useful when writing to privileged files.

Core Concepts Reinforced

touch → Create empty files

> → Overwrite file contents

>> → Append to existing content

cat → Display file content

head → View the beginning of a file

tail → View the end of a file

tee → Write to a file while displaying output

These commands form the foundation of everyday Linux file management and are essential skills for system administration.
