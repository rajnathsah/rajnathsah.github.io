---
title: "Useful Linux command and Shell script sniplets"
date: 2020-12-24 00:00:00 +0000
description: Useful linux command and shell script. # Add post description (optional)
img:  # Add image post (optional)
classes: wide
tags: [Shell Script, Linux command]
---
# Useful linux command
Set of common useful linux command with basic description and usage.  
* **ls** - List directory contents
  * ls  
  Prints all non hidden files and directory in current path.  
  * ls -a  
  Prints all files and directories (including hidden ones).  
  * ls -lrth  
  Prints all files and directories in ascending order.  
* **cat** - Print and concatenate file
  * cat filename  
  Prints the data of filename.
  * cat file1 > file2  
  Copies data of file1 to file2.
  * cat file1 file2 > file3  
  Redirect data of file1 and file2 to file3. 
* **touch** - Updates timestamp of the file and create empty file
  * touch filename  
  If filename is existing file then it updates the last modified timestamp to current timestamp, in case filename does not exist then it creates empty file.  
* **vi** - Open file in read only mode, if file does not exist then creates new one.  
  * vi filename  
  Open file in read only mode, use i to enter into edit mode, use esc :wq to save the change, use esc :q to quit the file without saving it. To search anything in file use vi <file name> then press / and type search keyword, to search for next occurence use n keyword.  
* **man** - Displays manual pages
  * man {command}  
  Display the manual pages of command.  
* **pwd** - Prints working directory
  * pwd  
  Display the full path of current working directory.  
* **cd** - Change directory
  * cd  
  Moves to home directory
  * cd directoryname  
  Change the directory to the given directoy.  
  * cd ..
  Changes one directory back
  * cd -
  Changes to previous directory path.  
* **rm** - Remove files and directory
  * rm filename
  Delete the filename file.  
  * rm -i filename  
  Deletes the file in interactive way.  
  * rm -r directory  
  Removes directory and its content recursively.  
  * rm -rf directory
  Remove directory and its content by force recursively.  
* **cp** - Copies files and directory
  * cp filename directory  
  Copies files to directory.  
  * cp -R source-directory target-directory  
  Copies data of source directory to target directory recursively.  
  * cp -pr filename new-filename  
  Copy data to new file while preserving timestamp, it can be used for taking backup.  
  * cp -pr directory new-directory  
  Create new copy of directory by coping data while preserving timestamp.  
* **mkdir** - Make directory
  * mkdir directory-name  
  Creates directory with directory name. 
  * mkdir -p directorty-1/directory 2  
  Creates parent and sub-directory if not present.  
  * mkdir directory-1 directory-2 directory-3
  Creates all 3 directory in current working directory.  
* **ps** - List current running processes
  * ps  
  List current running processes by logged in user.
  * ps -ef  
  List all running processes on system in full format.  
* **top** - Show real time processes  
* **kill** - Kill process with id pid  
  * kill pid Kill process with id pid  
  * pkill name Kill process with name name  
  * killall name Kill all processes with names beginning name  
* **scp** - Copies files between servers in secure way.
  * scp username@from_host:file.txt /local/directory/  
  Copy file from a remote host to local host.  
  * scp file.txt username@to_host:/remote/directory/  
  Copy file from local host to a remote host.  
  * scp -r username@from_host:/remote/directory/  /local/directory/  
  Copy directory from a remote host to local host.  
  * scp -r /local/directory/ username@to_host:/remote/directory/  
  Copy directory from local host to a remote host.  
  * scp username@from_host:/remote/directory/file.txt username@to_host:/remote/directory/  
  Copy file from remote host to remote host  
* **grep** - Searches text in file or text
  * grep 'word' filename  
  Search any line that contains the word in filename.  
  * grep -i 'bar' file1  
  A case-insensitive search for the word ‘bar’.  
  * grep -R 'foo' .  
  Search all files in the current directory and in all of its subdirectories for the word ‘foo’.
  * grep -c 'nixcraft' frontpage.md  
  Search and display the total number of times that the string ‘nixcraft’ appears in a file named frontpage.md.  
  * grep -w "boo" file  
  Searches whole word in file.  
  * egrep -w 'word1|word2' /path/to/file
  Searches 2 different words in file.  
  * grep -n 'root' /etc/passwd  
  Precede each line of output with the number of the line in the text file from which it was obtained.  
  * grep -l 'main' *.c
  List all files whose content mention main.  
* **chown** - Change the owner
  * chown owner-user file  
  Change ownership of file to owner-user.  
  * chown owner-user:owner-group file  
  Change ownership and group of file to owner-user and owner-group.  
  * chown owner-user:owner-group directory  
  Change ownership and group of directory to owner-user and owner-group.  
  * chown -R owner-user:owner-group directory  
  Change ownership and group of directory and sub directory recursively to owner-user and owner-group.  
* **df** - Displays the amount of available disck space for file system
  * df  
  Display disk usage.  
  * df -a  
  Display disk usage of all the file systems.  
  * df -h  
  Display disk usage in human readable format.  
  * df -T  
  Display the file system type in the output.  
  * df -h --total  
  Display the grand total of disk usage of all the file system.  
  * df --output={field_name1, field_name2}  
  Display the certain fields in df command output. Valid field names are: ‘source’, ‘fstype’, ‘itotal’, ‘iused’, ‘iavail’, ‘ipcent’, ‘size’, ‘used’, ‘avail’, ‘pcent’ and ‘target’.  
* **mv** - Move the file from one location to another or rename the file
  * mv -f source dest  
  Force move by overwriting destination file without prompt.  
  * mv -i source dest  
  Interactive prompt before overwrite.  
  * mv -u source dest  
  Move when source is newer than destination.  
  * mv -v source dest  
  Print source and destination files while moving.  
* **head** - Prints the first part of given file, by default it prints first 10 lines
  * head -line filename  
  Print number of line from filename.  
  * head -c-line filename  
  Print number of line character from filename.  
* **tail** - Prints the last part of given file, by default it prints last 10 lines 
  * tail -line filename  
  Print number of line from filename from end of file. 
  * tail -c-line filename  
  Print number of line character from filename from end of file.  
* **du** - Display the file/directory space usage
  * du -sh *
  Print size of the all directory and file inside the directory.

# Useful shell script sniplets
Set of useful shell script sniplets which are handy in day to work as database developer and admin.    
* Validate if directory exist or not
```shell
#!/bin/bash
dir='sample_dir'
if [ -d "$dir" ]
then
   echo "$dir found"
else
   echo "Error: $dir not found."
fi
```
Above command will fail if directory is a symbolic link. It can be handled differently using below option.
```shell
#!/bin/bash
dir='sample_dir'
if [ -d "$dir" ]
then
 if [ -L "$dir" ]
 then
   echo "$dir is symbolic link"
 else
   echo "$dir found"
 fi
else
   echo "Error: $dir not found."
fi
```
* List directory content with size
```shell
ls -lrth|grep G
```
* Truncate log files without deleting it
```shell
cat /dev/null > <filename>
```
* Archive log files to save space
```shell
tar -czvf <zip filename>.tar.gz <file name>
```
* Archive content of a directory and remove files
```shell
tar -czvf out_file.tar.gz --remove-files out_file
```
* Search given text in all files in a given path and prints it
```shell
find $fpath -type f -exec grep -H -i "$search" {} \;|awk -F ':' '{print $1}'|sort
```
* Remove duplicate lines from a file and save it in new file
```shell
sort <file name> | uniq > <new file name>
sort <file name> | uniq -u > <new file name>
#-u forces to check for strict ordering
uniq <file name> <output file name>
```
* Total size of files with specific names or extension
To check the size of all .txt file in current directory.
```shell
find . -type f -name "*.txt" -print0 | xargs -0r du -ch | tail -n1
```
# Question and Answer
1. How to execute another shell script from a shell script?  
* source /path/to/script or . /path/to/script
This option executes the another shell script in same process and all the variables made available in prior script is also available for the script being executed.  
* /bin/bash /path/to/script or ./path/to/script
This option executes the shell script in new process and so variables made available in prior script are not available.  

2. How to abort a shell script if any command returns a non zero value?
Use set command at begining of script
```shell
#!/bin/bash
set -e 
set -o pipefail
```
set -e : This works for all simple command.  
set -o pipefail : This work when pipe command are used.  

3. How to check if particular environment variable is set?
It can be checked using below command:
```shell
: "${<Environment Variable>:?Need to set <Environment Variable> non-empty}"
```

4. How to iterate over argument value passed to shell script?
Use "$@" to represent all arguments:
```shell
#!/bin/bash
for var in "$@"
do
   echo "$var"
done
```

5. How to get last argument passed to a shell script?
Using for loop where it keeps last value.
```shell
#!/bin/bash
for last; do true; done
echo $last
```

6. How to schedule a task in linux?  
Check if there is any cron job exist for user.  
```shell
crontab –l
```
Prepare a shell script or command and test it. Once it is finalized, schedule using cron. For scheduling cron job for specific user, edit the cron file using command.
```shell
crontab -e # for logged in user
crontab -e -u oracle # for specific user
```
Format:  
Seconds Minutes Hours Day_of_the_month Month Day_of_the_week Year <command/filename>  
Param details:  
![Cron Job]({{site.baseurl}}/assets/img/crontab_param.png)

Example to run a cleanup script called "/root/weekly_cleanup.sh" at 01:00 each Sunday. Any output or errors from the script are redirected to the "/tmp/weekly_cleanup.log" file, to prevent a build up of mails to user.
```shell
0 1 * * 0 /root/weekly_cleanup.sh >> /tmp/weekly_cleanup.log 2>&1
```

7. How to mount file system path to linux?  
Create a directory to which it needs to be mounted and then run sudo mount "File System path" "Local System path".
```shell
sudo mount /dev/sdb1 /mnt/media
```

8. How to find process running on specific port in linux?
List of running process on port can be find using lsof command.  
```shell
lsof -i :<port>
lsof -i :80
```

9. How to create and remove symbolic link in linux for any path?
```shell
# Create
ln -s <source path> <new path/link name>
# Remove
unlink <link name>
```
