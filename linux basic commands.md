## 1. Basic navigation commands

## Command Purpose

Show current directory

pwd

List files and directories

ls

Detailed file listing

ls -l

ls -a Show hidden files

ls -la Show all files with details

Go to root directory

cd /

Go to home directory

cd ~

cd .. Go one directory up

cd ../..

Go up two directories

cd folder Enter a directory

Return to previous directory

cd -

Clear terminal

clear

history

Show previously executed commands

Show current username

whoami

hostname

Show computer name

Display date and time

date

Display calendar

cal

## 2. Linux filesystem directories

These commands help you explore the Linux filesystem.

```
ls /
ls /bin
ls /sbin
ls /etc
ls /boot
ls /dev
ls /home
ls /root
ls /tmp
ls /var
ls /usr
ls /opt
ls /mnt
ls /media
```

## Important directories:

## Directory Purpose

/

Root of filesystem

/bin

Essential user commands


System administration commands

/sbin

/etc

Configuration files

/boot

Boot files and kernel

/dev

Device files

/home

Users' home directories

/root

Root user's home directory

/tmp

Temporary files

/var

Variable data, logs and caches

/usr

User applications and resources

/opt

Optional software

/mnt

Temporary mount points

/media

Removable media mount points

## 3. File and directory management

## Command

touch file.txt

mkdir folder

mkdir -p a/b/c

rmdir folder

cp file.txt copy.txt Copy a file

cp -r folder backup

mv file.txt new.txt

rm file.txt

rm -r folder

rm -i file.txt

file file.txt

stat file.txt

tree

## Purpose

Create an empty file

Create a directory

Create nested directories

Remove an empty directory

Copy a directory recursively

Rename or move a file

Delete a file

Delete a directory and contents

Ask before deleting

Identify file type

Display file metadata

Display directory structure

## Example:

mkdir devops

cd devops

mkdir linux

cd linux

touch notes.txt

cp notes.txt backup.txt

mv backup.txt linux-notes.txt

ls -l

Be careful

rm -rf can permanently delete files without confirmation. Never run it on a path you haven't checked.


## 4. Reading files

## Command

cat file.txt

less file.txt

more file.txt

head file.txt

head -n 5 file.txt First 5 lines

Last 10 lines

tail file.txt

tail -n 5 file.txt Last 5 lines

tail -f file.log

nl file.txt

wc file.txt

wc -l file.txt

wc -w file.txt

wc -c file.txt

## Purpose

Display complete file

Read file page by page

Read file page by page

First 10 lines

Follow a changing log

Display numbered lines

Count lines, words and bytes

Count lines

Count words

Count bytes

## 5. Creating and editing files

## Command

nano file.txt

vim file.txt

vi file.txt

echo "Hello"

echo "Hello" > file.txt

echo "Hello" >> file.txt Append text

Print formatted text

printf "Hello\n"

tee file.txt

## Purpose

Open Nano editor

Open Vim editor

Open Vi editor

Print text

Write text, replacing contents

Write terminal input to file

## Nano shortcuts:

Ctrl + O: Save

Ctrl + X: Exit


Ctrl + W: Search

Vim shortcuts:

- i: Insert mode

Esc: Normal mode

:w: Save

:q: Quit

:wq: Save and quit

- :q!: Quit without saving

dd: Delete line

yy: Copy line

p: Paste


## 6. Searching commands

## Command

## Purpose

```
grep "root" file.txt Search for text
grep -i "root" file.txt Case-insensitive search
grep -n "root" file.txt Show matching line numbers
grep -v "root" file.txt Show lines without the match
grep -r "root" folder/ Search recursively
grep -w "root" file.txt Match whole word
find . -name "file.txt" Find a file by name
find /home -type f Find regular files
find /home -type d Find directories
find . -name "*.txt" Find all text files
which ls Find executable location
whereis ls Find binary, source and manual locations
locate file.txt Search indexed file locations
```

## 7. Pipes and redirection

These are especially important for DevOps.

## Operator

| Operator | Purpose |
| --- | --- |
| > | Redirect output, overwrite file |
| >> | Append output |
| < | Take input from file |
| 2> | Redirect error output |
| 2>> | Append error output |
| \| | Send output to another command |
| &> | Redirect output and errors |

## Command

| Command | Purpose | Example |
| --- | --- | --- |
| less |   | Read file page by page; navigate freely less file.txt |
| more | Read file page by page Display first 10 lines | more file.txt |
| head |   | head file.txt |
| head -n 5 Display first 5 lines |   | head -n 5 file.txt |
| tail | Display last 10 lines | tail file.txt |
| tail -n 5 Display last 5 lines |   | tail -n 5 file.txt |
| tail -f | Monitor new log entries continuously | tail -f app.log |

## Key differences:


less vs more: Both read files page by page; less offers better navigation.

head: Shows the beginning.

tail: Shows the end.

DevOps tip: tail -f is commonly used to monitor application logs in real time.
