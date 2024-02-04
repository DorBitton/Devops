## Read and Use System Documentation
* `command --help`: Show help for a command.

* `man command` command: Display the command manual.
    * `man -k keyword`: Search the manual for the provided keyword.
    * `sudo mandb`: Create a database used by the man -k command.
    * `man -f printf`: Show man sections for the command, similar to whatis.

* `apropos command`: looks in man page and try to match the text we entered.
    * `sudo mandb`: Create a database used by the apropos command.
    * Commands will be found in section 1 and 8.

* `/usr/share/doc`: Contains configuration file examples.
* `info command`: Display info documentation. Example: `apropos -s 1,8 command`. Full list in `man man`.

* Bash Completion
    * During the digitalization of a command, press the Tab key twice to show possible values or parameters.
    * `yum -y install bash-completion`: Must be installed for Bash completion.


## Use input-output redirection (e.g. >, >>, |, 2>)

All Unix-based operating systems provide three primary input and output channels, known as stdin, stdout, and stderr. These channels facilitate communication between a program and its environment. In Bash, each channel is numbered from 0 to 2, representing stdin (0), stdout (1), and stderr (2). These channels are file descriptors, referring to specific files that can be manipulated, copied, read, or written.

Upon starting a Bash environment, the default descriptors point to the terminal where the session originated. Initially, stdin corresponds to user input, and both stdout for regular messages and stderr for error messages are directed to the terminal.

an active terminal within a Unix-based operating system is typically treated as a file, commonly stored in /dev/tty0. When a new session runs concurrently with an existing one, the subsequent terminal is assigned as /dev/tty1 and so forth. Consequently, initially, all three file descriptors point to the file symbolizing the terminal in which they are executed.
There are operator to redirect input, ouput and error.

* < - redirect stdin

  * `wc < file`

    Execute wc using the content of file as input

* \> and >> - redirect stdout 

  * `echo test > file1` - Writes "test" to file1, replacing its current content.

  * `echo test >> file1` - Appends "test" to file1.

* 2> - redirect stderr 

  * `find /proc -name "cpu*" 2> /dev/null` - Finds files/directories in /proc starting with "cpu" and redirects all errors, such as 'Permission Denied', to the virtual file /dev/null (discards all data).

* | - Pipe (stdout to stdin)

  * `cat file | wc` - Uses the output of cat file as input for the wc command.

* `2>&1` - Redirect stderr to the same location as stdout

* Combining Redirections: 

  *  `find /etc -name '\*a\*' 2> /dev/null | less` - Finds files in /etc with 'a' in their name, redirects errors to /dev/null, and pipes the output to the less command.

* `/dev/null` - In Unix-like operating systems, `/dev/null` is a special device file that discards all data written to it. It's often used as a sink for unwanted output or errors.
  * `find /directory -name "file" 2> /dev/null ` -  redirects standard error (stderr - file descriptor 2) to /dev/null.
  

## Managing Files and Directories in Linux

* Listing Files and Directories
  * `ls` - List directory contents.
    * `ls -l` - Display detailed information with additional columns: File Type, Permissions, Links, Owner, Group, Size, Creation Date, Creation Hour, and Name.
      First letter of first column indicate file type:
      * `-` : file
      * `d`: directory
      * `l`: link
    * `ls -la` - Show long output including hidden files.
    * `ls -lR` - Display long output recursively, showing subdirectories' content.
    * `ls -lt` - long output sorted by modification time.
    * `ls -ld /etc` - Show directory properties without displaying its content.

* Checking Disk Usage
  * `du file` - show disk usage.
    * `du directory` - show space used by directory and each subdirectory. It is recursive.
    * `du -s directory` - summarize space used by directory and subdirectory.
    * `du *` - show space of each file in current directory.
  * `pwd` - print current directory.

* File and Directory Creation
  * `touch file` - It creates an empty file.
  * `cp source destination` - copy source file to destination.
    * `cp file1 file2 ./dest` - Copy file2 and file2 to directory dest.
    * `cp * ./dest` - Copy all file of current directory to directory dest.
    * `cp -r dir1 dir2` - Copy dir1 in dir2. `-r` recursive.
    * `cp -a source destionation` - Will copy the file preserving it's permissions.
  * `mkdir dir` - create directory dir.
    * `mkdir -p dir/dir2` - Create a directory dir with a subdirecotory dir2.
  * `rmdir dir` - remove dir. Note: dir must be empty.
  * `tree` - show directories tree.
    * `yum -y install tree` -  Install the `tree` command if not available.
    
* File and Directory Manipulation
  * `mv file file2` - rename file in file2.
    * `mv file dir` - move file in directory dir.
    * `mv dir ..` - move directory dir at the upper directory level.
  * `rm file` - delete file.
    * `rm -f file` - remove read-only file.
    * `rm -r dir` - remove directory dir and all subdirectories and files.
  
* Additional Tips
    * pwd - Print the current directory.
    * Be cautious while using `rm -r` to avoid unintentional deletion of important files or directories.


## Create and manage hard and soft links

![inode](http://www.farhadsaberi.com/linux_freebsd/image/unix_file_system.gif)

The i-node (index node) is a data structure in a Unix-style file system that describes a file-system object such as a file or a directory. Each i-node stores the attributes and disk block location(s) of the object's data.

File-system object attributes may include metadata (times of last change, access, modification), as well as owner and permission data.

Directories are lists of names assigned to i-nodes. A directory contains an entry for itself, its parent, and each of its children.

Each i-nodes is identified by a unique i-node numbers.

*To summarize*: directory contains filenames, that is associated to i-node, that contains reference to data block.

*Hard link*

* The filenames is an hard link.

* I can have two filenames that point to same i-node.

* Hardlink limits:
  * Must point to same device
  * Hardlinks pointing a directory cannot be created

*Symbolic link*

* It's a pointer to a filename
* This means that there will by this chain: link -> filename -> i-node
  * If filename is removed, link will become invalid

* Note: permissions on a link are "open", because real permission are associate to i-node.

* `ls -li` - in first column show the i-node number.
* `ln target newname` - It will create and hard link to the same i-node of target with name (filename) newname.
* `ln -s target newlink` - It will create a symbolic link to target called newlink.
  * `ln -s /var .` - It will create a symbolic link to var in current directory. The name of link will be var.

**Note**: A file is considered deleted when there are no hard link to file's i-node. This means that `rm` remove link, hard or symbolic.

References:

* [https://en.wikipedia.org/wiki/Inode](https://en.wikipedia.org/wiki/Inode)

* [http://www.farhadsaberi.com/linux_freebsd/2010/12/files-directory-security-setuid-sticky-bit-permissions.html](http://www.farhadsaberi.com/linux_freebsd/2010/12/files-directory-security-setuid-sticky-bit-permissions.html)

* [http://www.compsci.hunter.cuny.edu/~sweiss/course_materials/unix_lecture_notes/chapter_03.pdf](http://www.compsci.hunter.cuny.edu/~sweiss/course_materials/unix_lecture_notes/chapter_03.pdf)


## Archive, backup, compress, unpack, and uncompress files

* `tar` - Save many files into a single file.

  `tar` uses `gzip`, `bzip2` and `xz` compression. `Gzip` is fast and more common, but it generally compresses a bit less. `Bzip2` is slower, but it compresses a bit more. `XZ` is the newest.

  File permissions are maintained by default only for file users. For other user I must explicit say to maintain permission during decompression using `-p` parameter

  * `tar jcfv file.tar.bz2 *` - Save all files of current directory in new bzip2 compressed file called file.tar.bz2

  * `tar jxfv file.tar.bz2` - Extract content of file.tar.bz2

  * `tar cvJf file.tar.xz *` - Create xz file. 

  * `tar czvf file.tar.gz *` - Create gzip file.

  * `tar tf file.tar` - Show content of file.tar.

  * `tar --delete -f test.tar file` - Delete file from test.tar. **Note**: test.tar isn't compressed.

  * `tar --update -f test.tar file` - Update file in test.tar. **Note**: test.tar isn't compressed.

  * `tar X<(command that generate list) -c -f file.tar *`

    `tar X<(ls | file -f - | grep -i MPEG | cut -d: -f 1) -c -f file.tar *`

    Exclude file MPEG from content of file.tar

  * `--same-permissions` - Keep the permissions.

  * `--acls` - preserve the ACL of the files.

* Backup a device

  `dd if=/dev/sda of=/system_images/sda.img` - Device must be unmounted.

* Restore device

  `dd if=/system_images/sda.img of=/dev/sda`

* `rsync` it is used to keep synchronized the content of two directories

  * `yum -y install rsync` - Install rsync command

  * `rsync -av source dest`

    Synchronize source with dest. `-a` archive, provide a series of default option

  * `rsync -avz /tmp user@123.123.123.123:/dest`

    Synchronize tmp with dest that it's contained in a remote machine with IP 123.123.123.123.

    `-z` means that content will be compressed during transfer

  * `rsync -avzhe ssh source root@remote_host:/remote_directory/`

    Synchronize source with remote_directory using ssh


## List, set, and change standard file permissions

To see user, group and permission use `ls -l`. Permissions are in the first column, name in third and group in fourth.

Each file/directory will have an *owner* and will be associated to a *group*.

The permissions for each file/directory are given for each of this category:

* Owner
* Group
* Others

Others are all other users that *are not the owner* and *are not member of group*.

For each category below permissions are valid:

* Read
  * Octal value: 4
* Write
  * Octal value: 2
* Exec (Execution)
  * Octal value: 1

The rights that each permission provide are different and depends if target is a file or a directory:

|           |     File     |   Directory   |
| :-------: | :----------: | :-----------: |
| Read (4)  | Read or Exec |   List (ls)   |
| Write (2) |    Modify    | Create/Delete |
| Exec (1)  |     Run      |      cd       |

**Note**: When exec is set for group on other section, file will be executed with identity of the user that are executing command (user ID) and group of user (group ID)

Absolute mode:

* Use numbers for each permission.

* `chmod 760 file` - Change file permission
  * Owner: grant read, write and exec
  * Group: grant read, write
  * Others: no permission

Relative mode:

* `chmod +x file` - Add exec to owner, group and other.
* `chmod g+w file` - Add write to group.
* `chmod o-rw file` - Remove read and write to others.


**Advanced permissions**

There are other special permissions that can be granted to file/directories

|                |         File         |                          Directory                          |
| :------------: | :------------------: | :---------------------------------------------------------: |
|    suid (4)    | Run as owner of file |                             N/A                             |
|    sgid (2)    |  Run as group owner  |       Inherit directory group when a file is created        |
| sticky bit (1) |         N/A          | A file can be deleted only by owner or by directory's owner |

* Suid: When a file with setuid is executed, the resulting process will assume the effective user ID given to the owner class. This enables users to be treated temporarily as root (or another user). E.g `passwd` has suid setted 
* Sgid:  When a file with *setgid* is executed, the resulting process will assume the group ID given to the group class
* Sticky bit is applied to /tmp

* Suid cannot be applied to Bash scripts

Absolute mode:

* `chmod 4760 file` - Change file permission
  - Add suid
  - Owner: grant read, write and exec
  - Group: grant read, write
  - Others: no permission

Relative mode:

* `chmod u+s file` - set suid
* `chmod g+s file` - set guid
* `chmod +t dir` - set sticky bit

References:

* [https://en.wikipedia.org/wiki/File_system_permissions#Changing_permission_behavior_with_setuid,_setgid,_and_sticky_bits](https://en.wikipedia.org/wiki/File_system_permissions#Changing_permission_behavior_with_setuid,_setgid,_and_sticky_bits)



## Manage access to the root account

* **root** is the system administrator.

* When logged as root, shell prompts `#` character. Otherwise `$`

* `su` - Used to become root. It will continue to use the current session with user and group id substituted.
  * It will ask root password.
* `su -` - Used to become root. It is same as logging into a fresh session on a terminal.
  * It will ask root password.
* `su - user` - Login as user.
  * It will be required user password.
  * If command is executed by root, password won't be required.

* `sudo -i` - ??? root login

* `sudo -l` - This will list all of the rules in the `/etc/sudoers` file that apply to your user. This gives you a good idea of what you will or will not be allowed to do with sudo as any user.

* `sudo` - command to allow an ordinary user to execute commands as a different user(usually the superuser).

* In default configuration, group `wheel` is authorized to act as root. If a user is member of `wheel` can execute all command as root with this syntax:
  * `sudo command`
  * **NOTE**: user password must be provided
* To add user to wheel execute:
  * `usermod -aG wheel username`

* `visudo` Modify the sudo configuration

  * Basic configuration:
  * ***demo*** ALL=(ALL:ALL)      ALL -> The first field indicates the username that the rule will apply to.
  - demo ***ALL***=(ALL:ALL)      ALL -> The first "ALL" indicates that this rule applies to all hosts.
  - demo ALL=(***ALL***:ALL)      ALL -> This "ALL" indicates that user demo can run commands as all users.
  - demo ALL=(ALL:***ALL***)      ALL -> This "ALL" indicates that user demo can run commands as all groups.
  - demo ALL=(ALL:ALL)      ***ALL*** -> The last "ALL" indicates these rules apply to all commands.

  With this row inserted in sudo configuration, demo user can execute this command:

  `sudo -u user command`

  This means that it will execute command with the identity of user.

  If `-u` is not specified, this means that command will be executed as root.

  demo user can open a root session running: `sudo su -`

  The powerfulness of this command is that a root session can be opened only providing user password (in this case the password of user demo).

  This means that root direct login (with user and password) could be disabled and root session will be opened using only `sudo`. Some Linux distribution use this method as default configuration (e.g Ubuntu).

  The advantage of this approach is that root password is **not shared** if I need to add a new system administrator.

* In sudo configuration `%` indicate group

  * %users  localhost=/sbin/shutdown -h now

    The users in group users can execute command /sbin/shutdown -h now on localhost as root.

* To simplify configuration in sudo configuration can be used alias

  *  Cmnd_Alias SOFTWARE = /bin/rpm, /usr/bin/up2date, /usr/bin/yum

    SOFTWARE can be used in sudo configuration rows.

* **Examples**:
  * Modify the sudo configuration to let the user `candidate` access root privileges with no password prompt.
      ```
      candidate ALL=(ALL) NOPASSWD: ALL
      ```
  * Ensure that all users can invoke the `last` command and access a list of users who previously logged in.
      ```
      ALL ALL=(ALL) /bin/last
      ```
  * Give the user `tim` sudo access to the `iptables` tools.
      ```
      tim ALL=/sbin/iptables, /path/other_command
      ```
  * User gacanepa to run /bin/updatedb without needing to enter his password.
    ```
    gacanepa ALL=NOPASSWD:/bin/updatedb
    ```

[Back to top of the page: ⬆️](https://github.com/StenlyTU/LFCS-official/blob/main/stuff/EssentialCommands.md#essential-commands)
