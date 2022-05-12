### What is inode? #flashcard #linux #filesystem
- All Unix files have its description stored in a structure called "inode" .
- An **inode** exists in, or on, a file system and represents metadata about a file
- The inode contains information about file :
    - size
    - location
    - time of last access
    - time of last modification
    - permission (owner, group)
    - except file name.
- The inode contains a list of pointer to the disk blocks that belong to that file or directory
- Disk block stores data for that inode
- Indodes are unique inside a files system
- Inode numbers are specific to a file system inside a disk partition. each files system has its own set of inode numbers.

---

### If inode does not have file name there where does it store? #flashcard #linux #filesystem

- Unix directories are lists of association structures, each of which contains one filename and one inode number.
- This technique called "**dentry"**
- A **dentry** is the glue that holds inodes and files together by relating inode numbers to file names.
- Directory's structure
    - A directory file is a list of directory entries, each one containing the following information:
    - **inode** - The inode for this directory entry. This is an index into the array of inodes held in the Inode Table of the Block Group. In figure 9.3, the directory entry for the file called file has a reference to inode number i1,
    - **name length** - The length of this directory entry in bytes,
    - **name** - The name of this directory entry.

![https://i.stack.imgur.com/Kj4NP.gif](https://i.stack.imgur.com/Kj4NP.gif)
---

### Where inodes table get stored? #flashcard #linux #filesystem

Physical disks contain a number of sectors. These are physical locations on the disk onto which data are stored. The physical drive is generally divided into partitions. These are logical separations of (logically) contiguous physical data storage area on the disk.

Partitions (or logical volumes) can contain filesystems (e. g. ext3fs, VFAT, hpfs). Each filesystem organizes itself in its own way with respect to keeping track of its organizational structure and whatnot. Many filesystems typically deployed onto Linux hosts (e. g. ext?fs) use inodes for this purpose or use an equivalent mechanism (e. g. reiserfs).

---

### Where the file system information get stored? #flashcard #linux #filesystem

its get stored in superblock

---

### What is Superblock? #filesystem

- The **superblock** is essentially file system metadata and defines the file system type, size, status, and information about other metadata structures (metadata of metadata).
- The superblock is very critical to the file system and therefore is stored in multiple redundant copies for each file system.
- The superblock is a very "high level" metadata structure for the file system.
- For example, if the superblock of a partition, /var, becomes corrupt then the file system in question (/var) cannot be mounted by the operating system. Commonly in this event, you need to run fsck which will automatically select an alternate, backup copy of the superblock and attempt to recover the file system.
    - The backup copies themselves are stored in block groups spread through the file system with the first stored at a 1 block offset from the start of the partition.
    - This is important in the event that a manual recovery is necessary. You may view information about ext2/ext3/ext4 superblock backups with the command `dumpe2fs /dev/foo | grep -I superblock` which is useful in the event of a manual recovery attempt.
    - lets suppose that the `dumpe2fs` command outputs the line Backup superblock at 163840, Group descriptors at 163841-163841. We can use this information, and additional knowledge about the file system structure, to attempt to use this superblock backup: `/sbin/fsck.ext3 -b 163840 -B 1024 /dev/foo`
    

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/8eb41ec3-0296-4532-a3b8-fdc42cffb559/Untitled.png)

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/6ba4422a-9a0b-418f-a5b3-2dee557c18ec/Untitled.png)

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/24463c58-b1b4-4f30-95be-76fa8cb35e74/Untitled.png)

---

### What is block ? #flashcard #linux #filesystem

When a partition or disk is formatted, the sectors in the hardisk is first divided into small groups. This groups of sectors is called as blocks. The block size is something that can be specified when a user formats a partition using the command line parameters available.

- Block Size for Ext2 can be 1Kb, 2Kb, 4Kb, 8Kb
- Block Size for Ext3 can be 1Kb, 2Kb, 4Kb(default), 8Kb
- Block Size for Ext4 can be 1Kb to 64Kb

The block size you select will impact the following things

- Maximum File Size
- Maximum File System Size
- Performance

The reason block size has an impact on performance is because, the file system driver sends block size ranges to the underlying drive, while reading and writing things to file system. Just imagine if you have a large file, reading smaller blocks (which combined together makes the file size) one by one will take longer. So the basic idea is to keep bigger block size, if your intention is to store large files on the file system.

Less IOPS will be performed if you have larger block size for your file system.

And if you are willing to store smaller files on the file system, its better to go with smaller block size as it will save a lot of disk space and also from performance perspective.

---

### What do you mean a File System? #flashcard #linux #filesystem

- File System is a method to store and organize files and directories on disk
- A file system can have different formats called file system types. These formats determine how the information is stored as files and directories.

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/386248ae-6865-4485-9dc6-c166abf1659b/Untitled.png)

---

### Difference between ext2 and ext3 file system #flashcard #linux #filesystem

- The ext3 file system is an enhanced version of the ext2 file system.
- Ext3 supports journaling
- After an unexpected power failure or system crash (also called an unclean system shutdown), each mounted ext2 file system on the machine must be checked for consistency by the e2fsck program. This is a time-consuming process and during this time, any data on the volumes is unreachable.
- The journaling provided by the ext3 file system means that this sort of file system check is no longer necessary after an unclean system shutdown. The only time a consistency check occurs using ext3 is in certain rare hardware failure cases, such as hard drive failures. The time to recover an ext3 file system after an unclean system shutdown does not depend on the size of the file system or the number of files; rather, it depends on the size of the journal used to maintain consistency.
- The default journal size takes about a second to recover, depending on the speed of the hardware.

---

### Any idea about ext4 file system? #flashcard #linux #filesystem

- Ext4 filesystem released as a functionally complete and stable filesystem in Linux with kernel version 2.6.28.
- Currently, Ext3 supports 16 TB of maximum file system size and 2 TB of maximum file size. Ext4 have 1 EB of maximum file system size and 16 **TB** of maximum file size.
    - 1024 Terabytes = 1 Petabyte
    - 1024 Petabytes = 1 Exabyte
    - 1024 Exabytes = 1 Zettabyte
- You can disable journaling on ex4 files system.

---

### Explain /proc filesystem? #flashcard #linux #filesystem

- `/proc` is a virtual filesystem that provides detailed information about Linux kernel, hardware’s and running processes.
- Files under `/proc` directory named as Virtual files. Because `/proc` contains virtual files that’s why it is called virtual file system.
- These virtual files have unique qualities. Most of them are listed as zero bytes in size.
- Virtual files such as `/proc/interrupts` , `/proc/meminfo` , `/proc/mounts` , and `/proc/partitions` provide an up-to-the-moment glimpse of the system's hardware. Others, like the `/proc/filesystems` file and the `/proc/sys/` directory provide system configuration information and interfaces.

---

### Can we change files parameters placed under /proc directory? #flashcard #linux #filesystem

- Yes
- To change the value of a virtual file, use the echo command and a greater than symbol (`>` ) to redirect the new value to the file. For example, to change the hostname on the fly, type:
- `echo www.xyz.com > /proc/sys/kernel/hostname`

---

### What is the use of sysctl command? #flashcard #linux #filesystem

The `/sbin/sysctl` command is used to view, set, and automate kernel settings in the `/proc/sys/` directory.

---

### /proc/ directory contains a number of directories with numerical names. What is that? #flashcard #linux #filesystem

These directories are called process directories, as they are named after a program's process ID and contain information specific to that process.

---

### What is Library? #flashcard #linux 

- A library is a collection of functions.
- We are using functions which are not defined in our code, or in that particular file.
- To have access to them, we include a header file, that contains declarations of those functions.
- After compile, there is a process called linking, that links those function declarations with their definitions, which are in another file. The result of this is the actual executable file.

---

### Difference between static linking and dynamic linking? #flashcard #linux

- **Static linking** means that every executable file contains, every library (collection of functions) that it needs. This is a waste of space, as there are many programs that may need the same functions. In this case, in memory there would be more copies of the same function.
- **Dynamic linking** prevents this, by linking at the run-time, not at the compile time. This means that all the functions are in a special memory space and every program can access them, without having multiple copies of them. This reduces the amount of memory required.

---

### Difference between Hard links vs Soft links? #flashcard #linux #filesystem #file

- Underneath the file system, files are represented by inodes.
- A file in the file system is basically a link to an inode.
- A hard link, then, just creates another file with a link to the same underlying inode.
- When you delete a file, it removes one link to the underlying inode. The inode is only deleted (or deletable/over-writable) when all links to the inode have been deleted.
- The directories cannot be hard linked. Linux does not permit this to maintain the acyclic tree structure of directories.
- A hard link cannot be created across filesystems. Both the files must be on the same filesystems, because different filesystems have different independent inode tables (two files on different filesystems, but with same inode number will be different).
- Hard link point to the file content. while Soft link points to the file name.
- Hard links share the same inode. Soft links do not.
- Hard links can't cross file systems. Soft links do.

---

### Why can't we create Hardlink on Directory ? #flashcard #linux #filesystem #file

- The system uses hard links to manage the link from the parent to the directory's inode, the . link in that directory and all of the .. entries in all of its subdirectories.
- USER'S (including 'root') are forbidden to create additional hard links because this would make fsck much more difficult to implement and it might allow one to create hard link loops, and dangling sub trees.
- In other words it's a policy enforced by the kernel.
- Linux does allow hard links to directories, but it only allows hard links to a directory from itself and it's parent directory. These are the two hard links to which you refer.


---

### File Descriptors  #flashcard #linux #file

1. `File Descriptors` (FD) are non-negative integers `(0, 1, 2, ...)` that are associated with files that are opened.
2. `0, 1, 2` are standard **FD**'s that corresponds to `STDIN_FILENO`, `STDOUT_FILENO` and `STDERR_FILENO` (defined in `unistd.h`) opened by default on behalf of shell when the program starts.
3. FD's are allocated in the sequential order, meaning the lowest possible unallocated integer value.
4. FD's for a particular process can be seen in `/proc/$pid/fd` (on Unix based systems).

---

### What is SUID? #flashcard #linux #permissions

- Set UID means, if user ABC run FOO binary, it will not run as user ABC, instead it will run as owner of that binary.
- For example `passwd` binary has set SUID, if anyone run `passwd` command it will run as root user.
- To Set SUID, we can use `chmod 4755 FOO` or `chmod u+s FOO`

---

### What is SGID? #flashcard #linux #permissions

- Set GID is set on directory, so any new files get created inside directory, it will inherits the group permission to the new files.
- To set SGID, we can use `chmod 2755 FOODIR` or `chmod g+s FOODIR`

---

### What is Sticky Bit? #flashcard #linux #permissions

- Sticky bit is useful for security feature when used with directory.
- In Linux all users allow to create files under `/tmp/` but none of can delete files not owned by him. that's possible because of sticky bit.
- To set sticky bit, we can `chmod +t FOODIR` or `chmod 1755 FOODIR`

---

### What is difference between small t and capital T in Linux permission? #flashcard #linux #permissions

- `t` For directory with executable permission
- `T` for directory with non-executable permission

---

### What is UMASK?  #flashcard #linux #permissions

- User file creation mode is used to determine the file permission for newly created files.
- In short, default permission get assign to file/dir using umask. default is `002`
- With this mask default directory permissions are `775` and default file permissions are `664`.
1. The default **umask for the root user is `022`** result into default directory permissions are `755` and default file permissions are `644`.
2. For directories, the **base permissions** are (`rwxrwxrwx`) `0777` and for files they are `0666` (`rw-rw-rw)`.


---

### How to restrict any user including root from making any changes to particular file?  #flashcard #linux #permissions

- The `chattr` command is used to change file attributes on a Linux file system.
- e.g. to make file read-only: `chattr +i file`
- If anyone try to write or delete this file they will get error: `Operation not permitted`

---

### What is the + plus sign you see at the end of permission for some directories?  #flashcard #linux #permissions

- `+` means that the file has additional ACLs set
- You can set them with `setfacl` and query them with `getfacl`

```bash
martin@martin ~ % touch file
martin@martin ~ % ll file 
-rw-rw-r-- 1 martin martin 0 Sep 23 21:59 file
martin@martin ~ % setfacl -m u:root:rw file 
martin@martin ~ % ll file 
-rw-rw-r--+ 1 martin martin 0 Sep 23 21:59 file
martin@martin ~ % getfacl file 
# file: file
# owner: martin
# group: martin
user::rw-
user:root:rw-
group::rw-
mask::rw-
other::r--

```

---

### How do you give acl in Linux?  #flashcard #linux #permissions

Using `setacl` command

---



