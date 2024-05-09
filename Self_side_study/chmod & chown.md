## Change Mode (chmod) Meaning and Purpose

The chmod or change mode allows an administrator to set or modify a file permission. Every Linux file has an owner group attached to it, and every file has permission associated with it.

Linux systems have many users and a user may be referred to as an individual or system operation. UNIX/Linux identifies each user with a UID. Users may be further organized into groups.

### Change Mode syntax and mode parameters

The syntax of the chmod command line is:
```
chmod mode file
```
Example
```
Chmod 720 readme.txt
```
**Where each number in the mode of parameters represents permissions for a user or group of users:**
- The first number represents the file owner.
- The second number represents the file’s group.
- The third number represents everyone else’s.

**There are also three basic permissions:**
- **Read** **(r):** Only allows the content of a file to be viewed.
- **Write**  **(w):** Allows content to be viewed, edited/modified, or deleted.
- **Execute** **(x):** Allows a content or file to be executed.

**Generally implemented options are:**
- -R: It stands for recursive, i.e., add objects to subdirectories.
- -V: It stands for verbose, display objects modified (unmodified objects are not displayed).
The target object is influenced if a symbolic link is mentioned. File modes related to symbolic links themselves directly are not used typically.

## The chown command
This allows you to change a user or group ownership of a given file, directory, or symbolic link.

**How to use chown command**
Let's start by reviewing basic syntax. The chown command syntax takes the following form:
```
chown [OPTIONS] USER[:GROUP] FILE(s)
```
- USER is the user name or the user ID (UID) of the new owner. GROUP is the new group’s name or the group ID (GID). FILE(s) is the name of one or more files, directories, or links. Numeric IDs should be prefixed with the + symbol.
- USER - If only the user is specified, the specified user will become the owner of the given files. The group ownership has not changed.
- USER: - When the username is followed by a colon :, and the group name is not given, the user will become the owner of the files, and the files group ownership is changed to the user’s login group.
- USER:GROUP - If both the user and the group are specified (with no space between them), the user ownership of the files is changed to the given user, and the group ownership is changed to the given group.
- :GROUP - If the User is omitted and the group is prefixed with a colon :, only the group ownership of the files is changed to the given group.
- : If only a colon : is given, without specifying the user and the group, no change is made.
By default, chown doesn’t produce any output on success and returns zero.

To find out who owns a file or what group the file belongs to, use the ls -l command:
```
ls -l filename.txt
```
```
-rw-r--r-- 12 linuxize users 12.0K Apr  8 20:51 filename.txt
|[-][-][-]-   [------] [---]
                |       |
                |       +-----------> Group
                +-------------------> Owner
```

**How to Change the Owner of a File**
To change the owner of a file, use the chown command followed by the user name of the new owner and the target file as an argument:
```
chown USER FILE
```
For example, the following command will change the ownership of a file named file1 to a new owner named linuxize:
```
chown linuxize file1
```
To change the ownership of multiple files or directories, specify them as a space-separated list. The command below changes the ownership of a file named file1 and directory dir1 to a new owner named linuxize:
```
chown linuxize file1 dir1
```
The numeric user ID (UID) can be used instead of the username. The following example will change the ownership of a file named file2 to a new owner with a UID of 1000:
```
chown 1000 file2
```
**How to Change the Owner and Group of a File**
To change both the owner and the group of a file use the chown command followed by the new owner and group separated by a colon (:) with no intervening spaces and the target file.
```
chown USER:GROUP FILE
```
The following command will change the ownership of a file named file1 to a new owner named linuxize and group users:
```
chown linuxize:users file1
```
If you omit the group name after the colon (:), the group of the file is changed to the specified user’s login group:
```
chown linuxize: file1
```

**Conclusion**
chown is a Linux/UNIX command-line utility for changing the file’s user and group ownership.

