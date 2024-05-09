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


