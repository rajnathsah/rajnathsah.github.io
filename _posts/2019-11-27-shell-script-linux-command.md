---
layout: post
title: "Linux command and Shell script"
date: 2019-11-27 00:00:00 +0000
description: Useful linux command and shell script. # Add post description (optional)
img:  # Add image post (optional)
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
* **man** - Displays manual pages
  * man <command>  
  Display the manual pages of command.  
* **pwd** - Prints working directory
  * pwd  
  Display the full path of current working directory.  
* **cd** - Change directory
  * cd  
  Moves to home directory
  * cd <directory name>  
  Change the directory to the given directoy.  
  * cd ..
  Changes one directory back
  * cd -
  Changes to previous directory path.  
* **rm** - Remove files and directory
  * rm <filename>
  Delete the filename file.  
  * rm -i <filename>  
  Deletes the file in interactive way.  
  * rm -r <directory>  
  Removes directory and its content recursively.  
  * rm -rf <directory>
  Remove directory and its content by force recursively.  
* **cp** - Copies files and directory
  * cp <filename> <directory>  
  Copies files to directory.  
  * cp -R <source directory> <target directory>  
  Copies data of source directory to target directory recursively.  
  * cp -pr <filename> <new filename>  
  Copy data to new file while preserving timestamp, it can be used for taking backup.  
  * cp -pr <directory> <new directory>  
  Create new copy of directory by coping data while preserving timestamp.  
* mkdir - Make directory
* ps - List current running processes
* scp - Copies files between servers in secure way.
* grep - Searches text in file or text
* chown - Change the owner
* df - Displays the amount of available disck space for file system
* mv - Move the file from one location to another or rename the file
* head - Prints the first part of given file, by default it prints first 10 lines
* tail - Prints the last part of given file, by default it prints last 10 lines 
* du - Display the file/directory space usage

# Useful shell script sniplets
Set of useful shell script sniplets which are handy in day to work as database developer and admin.    
* Validate if directory exist or not
```shell
#!/bin/bash
dir='sample_dir'
if [ -d "$dir" -a ! -h "$dir" ]
then
   echo "$dir found"
else
   echo "Error: $dir not found."
fi
```
