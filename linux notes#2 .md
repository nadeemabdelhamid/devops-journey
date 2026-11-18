## Linux Lecture 2 – Complete Explanation

1.

I'll explain each topic step by step, using practical scenarios and terminal commands, especially from a DevOps perspective.

## Section 1: Users and Groups

## 1. What are users in Linux?

Linux is a multi-user operating system. This means multiple people and services can use the same Linux machine.

Imagine a company has one Linux server used by three developers:

John – Backend developer.

Ahmed – Frontend developer.

Sarah – DevOps engineer.

Each person needs a separate account with specific permissions.

Linux uses a UID (User ID) to identify each user.

There are two main types of users:

## A. Normal users

These are human users who interact with the system.

Examples:


john

ahmed

sarah

Usually:

UID starts from 1000.

Have a home directory, such as /home/john.

Can log in normally.

- B. System users

These accounts are created to run system services.

Examples:

daemon

syslog

www-data


Usually:

UID is below 1000 (depending on the distribution).

Often cannot log in interactively.

Used to run specific services.

## Real-world scenario:

Suppose you install Nginx on Ubuntu.

Instead of running Nginx as your personal user, Linux can run it under a dedicated service account such as www-data.

## Why?

IfNginx gets compromised, the attacker gets the privileges of that service account rather than automatically gaining full system access.

This follows the principle of least privilege.

## 2. How to check your current user?

id

## Example:

```
uid=1001(john) gid=1001(finance) groups=1001(finance),1002(accounting)
```

Let's understand:

## Output Meaning

uid=1001 User ID

john

gid=1001 Primary Group ID

finance

Username

Primary group


accounting Secondary group

You can also use:

whoami

This prints your current username.

## 3. Why do we have groups?

Imagine a company has five developers working on the same project.

You want all developers to access:

```
/project
```

Without groups, you would need to manage permissions for every developer individually.

Instead, create a group:

```
sudo groupadd developers
```

Then add users to it:

```
sudo usermod -aG developers john
sudo usermod -aG developers ahmed
sudo usermod -aG developers sarah
```

Now all three users belong to the developers group.

Best Practices for Securing Remote Access to Linux Servers 2026 linuxsecurity.com [URL 🔗](https://images.openai.com/static-rsc-4/oHVD4Q-l6LVXtaz0VNQLanNRp-JWulvOnXFoYWAkxUl1Z01vfZ5GTmQhKBluJ9Gfx_1b7VcdVnWXIhFbq5hDcA7blE4czIJVKMrL8qeRio_tPygCI30qhFbSRdTWzj1yVRCTHFC0hMTZNS4T8cqVDUurJQJfZa2nCkAZBpNLDuU?purpose=inline)

## Primary group vs Secondary group

Primary group: The default group associated with the user. New files typically inherit this group, subject to directory settings.

Secondary group: Additional groups that give the user access to shared resources.

Example:


```
id john
uid=1001(john) gid=1001(finance)
groups=1001(finance),1002(accounting),1003(developers)
Here:
finance = primary group.
accounting and developers = secondary groups.
```

## 4. Creating users

Let's create a user named John.

## First, create a group:

sudo groupadd finance

Create another group:

```
sudo groupadd accounting
```

Now create John:

```
sudo useradd -m -s /bin/bash -g finance -G accounting john
```

Let's understand every option:

## Option Meaning

Create home directory

-m

Specify default shell

-s

Set primary group

-g

Set secondary groups

-G

Set his password:

```
sudo passwd john
```

Important: sudo allows an authorized user to execute a command with elevated privileges.

## 5. Important Linux user files


## Your lecture introduces three important files.

## /etc/passwd

Stores user account information.

## Example:

john:x:1001:1001:John Smith:/home/john:/bin/bash

## Field

john

x

1001

1001

John Smith Full name

/home/john Home directory

/bin/bash Default shell

Notice that x does not contain the actual password.

## Meaning

Username

Password placeholder

UID

GID

## /etc/shadow

Stores password hashes and password expiration information.

## Example:

john:\$6\$df3...\$:19740:0:90:10:::

It is restricted because it contains sensitive authentication information.

## /etc/group

Stores group information.

## Example:

finance:x:1001:john,ben

This means:

Group name: finance.

GID: 1001.


Members: john and ben.

## 6. Modifying and deleting users

Add John to another group:

```
sudo usermod -aG accounting john
Important: Why -aG and not just -G?
```

-G replaces the secondary group list.

-aG appends the new group without removing existing secondary groups.

## Change a group name:

```
sudo groupmod -n finance-team finance
```

## Delete a user:

```
sudo userdel john
```

Delete a user and their home directory:

```
sudo userdel -r john
```

## Delete a group:

```
sudo groupdel finance
```

## 7. Password expiration – chage

Imagine you work in a company where employees must change their passwords every 90 days.

Linux provides:

```
chage
```


Example:

```
sudo chage -m 7 -M 90 -W 10 john
```

Option

Meaning

-m 7

Minimum 7 days between password changes

-M 90

Password valid for maximum 90 days

-W 10

Warn 10 days before expiration

Account expiration date

-E

Display current settings

-l

Check John's settings:

sudo chage -l john

## Section 2: File Permissions and Ownership

This is one of the most important Linux topics.

## 1. Why do we need permissions?

Imagine a server containing:

```
/project
app.py
database.conf
deploy.sh
```

Three people access this server:

John: Developer.

Ahmed: Developer.

Sarah: System administrator.

Should everyone be allowed to modify database.conf?


Probably not!

Linux uses permissions to control who can read, modify, and execute files.

## 2. Understanding permissions

Every file has three categories:

1.

User (owner)

2.

3.

Group

4.

5.

Others

6.

And three permission types:

## Permission Symbol Meaning

Read

Write

Execute

Consider:

-rwxr-xr--

r

Read file contents

w

Modify file contents

x

Execute a file

## Understanding the permission string

-

File type

rwx

Owner

r-x


Group

r--

Others

\- = Regular file.

Owner can read, write and execute.

Group can read and execute.

Others can only read.

To see permissions:

ls -l

Example:

```
-rwxr-xr-- 1 john developers 1200 Nov 17 app.sh
```

## 3. Changing permissions – chmod

There are two methods.

## A. Symbolic method

You can change permissions using letters.

```
chmod u+x script.sh
```

Meaning: Give the owner execute permission.

Other examples:

```
chmod g-w report.txt
```


Remove write permission from the group.

Set others' permissions to read-only.

## The symbols:

## Symbol Meaning

- u g o a + - = Add permission

- Group

- Others

All

- Remove permission

- Set exact permissions

- User/owner

## B. Numeric method (Octal)

This method is particularly important for Linux and DevOps.

Each permission has a number:

## Permission Number

Read (r) 4

Write (w) 2

Execute (x) 1

You add the numbers together.

## Examples

7

r-x = 4 + 0 + 1

5

rw- = 4 + 2 + 0

6


r-- = 4 + 0 + 0

## 4

## Example: chmod 755 script.sh

The three numbers represent:

First number: Owner.

Second number: Group.

Third number: Others.

Therefore:

## Number Permissions

7 5 5

r-x

rwx r-x

## Result:

-rwxr-xr-x

## More important examples

## Command Owner Group Others

chmod 777 file rwx rwx rwx chmod 755 file rwx r-x r-x chmod 700 file rwx --- --- chmod 644 file rw- r-- r--

chmod 600 file rw- --- ---

## DevOps example:


## You have a private SSH key:

```
chmod 600 private_key.pem
```

## Why?

Only the owner should be able to read and write the private key.

## Recursive permissions

```
chmod -R 755 /var/www
```

-R means recursive.

It applies permissions to the directory and everything inside it.

Be careful: applying 755 recursively to a directory tree can make ordinary files executable unnecessarily.

## 4. Changing ownership – chown

Permissions determine what someone can do.

Ownership determines who owns the file and which group it belongs to.

## Example:

sudo chown john file.txt

Changes the owner to John.

```
sudo chown :finance file.txt
```

Changes the group to finance.

```
sudo chown john:finance file.txt
```

## Changes both owner and group.

## Recursive:

sudo chown -R john:finance /data

## Practical scenario

You have a web server directory:

```
/var/www/project
```

You want:


Owner: john.

Group: developers.

Owner can read, write and execute.

Group can read and execute.

Others can only read and execute.

Commands:

sudo chown john:developers /var/www/project sudo chmod 755 /var/www/project

## Section 3: Process Management

Now we move to another important topic.

## 1. What is a process?

A process is a program that is currently running.

For example, when you execute:

```
python3 app.py
```

Linux creates a process to run that Python program.

Every process has a unique identifier called:

## PID = Process ID


Imagine your Linux server runs:

Nginx.

MySQL.

Python application.

SSH service.

Each running process has its own PID.

## 2. Viewing processes – ps

The ps command displays information about running processes.

Basic:

ps

Shows processes associated with your current terminal.

Show processes across users:

ps aux

Meaning:

## Option

## Meaning

a u

Processes associated with terminals across users

User-oriented format

x

Include processes without a controlling terminal

## Example:

```
ps aux | grep nginx
```


This searches the process listing for Nginx.

The pipe | sends the output of the first command to the second command.

Another command:

Provides a detailed process listing.

## 3. Real-time monitoring – top

Imagine your server becomes slow.

You suspect that a process is consuming too much CPU or RAM.

Run:

top

It displays live information about processes and system resource usage.

[How to View Running Processes on Linux](https://images.openai.com/static-rsc-4/itDJfVajUisDOZkR9b7cZ-dPEv0eMZdUqzcdT7REgoc_TpJ1rTvQEBNr3POvj0x5VY--IRktpo98lljxsuWH8V3w9-x7DMVrNJjF9DFG_8mRskIEj8VB4R5NDH5DqNSGI8RLjBLYXH60eCWEZIc46--xxmirZTYCpqXNEHvUpq8?purpose=inline)

[howtoforge.com](https://www.howtoforge.com/how-to-view-running-processes-on-linux/?utm_source=chatgpt.com)

Useful shortcuts:

Key

Show CPU usage per core

1

Change refresh interval

s

Help

h

Kill a process by PID

k

Shift + M Sort by memory

Shift + P Sort by CPU

Save configuration

w

Function

## htop

An enhanced, interactive alternative to top.

## Install it:

```
sudo apt install htop
```

## Run:

htop


## 4. Process hierarchy – pstree

Linux processes can create other processes.

This creates a parent-child relationship.

For example:

systemd

├── sshd

├── nginx

└── cron

View the process tree:

The -p option displays PIDs.

Other useful commands:

Find processes belonging to John.

```
pidof nginx
```

Find the PID ofNginx.

## 5. Job control – Foreground and Background

This is an important concept.

Imagine you execute:

The terminal waits until the command finishes.

This is called running in the foreground.

But what if you want to run another command without waiting?

Use:

The & runs the command in the background.


Check background jobs

jobs

Bring a background job to the foreground

fg %1

Here %1 refers to job number 1.

Pause a running process

Press:

Ctrl + Z

This suspends the foreground job.

Resume in the background

bg %1

## Understand the difference

Foreground

Terminal is occupied by the running command.

Background

Command runs while you can continue using the terminal.

Suspended

Process is stopped temporarily until resumed.

## 6. Killing processes – Signals

Sometimes a process hangs or consumes too many resources.

You may need to terminate it.

Linux uses signals to communicate with processes.

View available signals:

kill -l


Three important signals:

## Signal Number SIGINT 2 Meaning

Interrupt

SIGTERM 15

SIGKILL 9

SIGSTOP 19

Request graceful termination

Force termination

Stop process

## SIGTERM

kill 1234

By default, kill sends SIGTERM.

It asks the process to terminate gracefully.

## SIGKILL

kill -9 1234

Immediately forces the process to terminate. The process cannot catch or ignore this signal.

Best practice: Try SIGTERM first. Use SIGKILL only when necessary.

## Kill by process name

pkill nginx

Terminates matching Nginx processes that you have permission to signal.

killall nginx

Terminates processes with the specified name.

## Kill processes belonging to a user

```
sudo pkill -u john
```

Terminates processes belonging to John (subject to permissions and process behavior).

## Final Lecture Summary

Commands to remember

useradd, usermod, userdel, id groupadd, groupmod, groupdel

Users

Groups

## Topic


## Passwords User information Group information /etc/group Permissions Ownership Process monitoring ps, top, htop Process hierarchy Job control Terminating processes kill, pkill, killall passwd, chage /etc/passwd, /etc/shadow chmod chown pstree, pgrep, pidof jobs, fg, bg
