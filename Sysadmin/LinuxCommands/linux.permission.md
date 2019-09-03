# Unix Permissions: File Permissions in Unix with Examples
## Unix Permissions: File Permissions with Examples
### Access to a file has three levels:

- Read permission – If authorized, the user can read the contents of the file.
- Write permission – If authorized, the user can modify the file.
- Execute permission – If authorized, the user can execute the file as a program.

### Each file is associated with a set of identifiers that are used to determine who can access the file:

- User ID (UID) – Specifies the user that owns the file. By default, this is the creator of the file.
- Group ID (GID) – Specifies the user-group that the file belongs to.

### Finally, there are three sets of access permissions associated with each file:

- User permission – Specifies the level of access given to the user matching the file’s UID.
- Group permission – Specifies the level of access given to users in groups matching the file’s GID.
- Others permission – Specifies the level of access given to users without a matching UID or GID.

Together, this scheme of access controls makes the Unix system extremely secure while simultaneously providing the flexibility required of a multi-user system.

The ls -l command can be used to view the permissions associated with each of the files in the current folder.

Example output of this command is given below.

*Example:*
**flags links owner group size modified-date name**

```
total of 24

drwxr-xr-x 7 user staff 224 Jun 21 15:26 .
drwxrwxrwx 8 user staff 576 Jun 21 15:02.
-rw-r--r-- 1 user staff 6 Jun 21 15:04 .hfile
drwxr-xr-x 3 user staff 96 Jun 21 15:17 dir1
drwxr-xr-x 2 user staff 64 Jun 21 15:04 dir2
-rw-r--r-- 1 user staff 39 Jun 21 15:37 file1
-rw-r--r-- 1 user staff 35 Jun 21 15:32 file2
```
In this output, the ‘total 24’ indicates the total number of blocks occupied by the listed files.

#### **The remaining columns are:**

- flags – A collection of flags indicating the file mode and the file permissions.
- links – The number of links associated with the file.
- owner – The UID that owns the file.
- group – The GIDs associated with the file.
- size – The size of the file in bytes.
- modified-date – The month, date, hour and minute of the last modification to the file.
- name – The name of the file or directory.

### The flags in the first column specify the file mode and the different sets of permissions:
* **The first character indicates the type of file:**
    - – : represents an ordinary file
    - d: represents a directory
    - c: represents a character device file
    - b: represents a block device file
* **The next three characters indicate user permissions:**
    * **The first of these three indicates whether the user has read permission:**
        - – : indicates that the user does not have read permission.
        - r: indicates that the user has read permission.
    * **The second character indicates whether the user has to write permission:**
        - – : indicates the user does not have write permission.
        - w: indicates the user has to write permission.
    * **The last character indicates whether the user has executed permission:**
        - – : indicates that the user does not have to execute permission.
        - x: indicates that the user has executed permission.

* The next three characters indicate group permissions, similar to the user permissions above.
* The final three characters indicate public permissions, similar to the user permissions above.

In case the file is an ordinary file, read permission allows the user to open the file and examine its contents. Write permission allows the user to modify the contents of the file. And execute permission allows the user to run the file as a program.

In case the file is a directory, read permission allows the user to list the contents of the directory. Write permission allows the users to create a new file in the directory, and to remove a file or directory from it. Execute permission allows the user to run a search on the directory.

### Unix command line tools to change the access permissions
#### Unix provides a number of command line tools to change the access permissions:

> **Note** that only the owner of the file can change the access permissions.

1. chmod: change file access permissions

    * **description:** This command is used to change the file permissions. These permissions are read, write and execute permission for the owner, group, and others.
    * **syntax (symbolic mode):**
    ```chmod [ugoa][[+-=][mode]] file```

    * The first optional parameter indicates who – this can be (u)ser, (g)roup, (o)thers or (a)ll
    * The second optional parameter indicates opcode – this can be for adding (+), removing (-) or assigning (=) a permission.
    * The third optional parameter indicates the mode – this can be (r)ead, (w)rite, or e(x)ecute.

    **Example:** Add write permission for user, group and others for file1
    ```
    $ ls -l
    -rw-r–r– 1 user staff 39 Jun 21 15:37 file1
    -rw-r–r– 1 user staff 35 Jun 21 15:32 file2
    ```
    ```
    $ chmod ugo+w file1

    $ ls -l
    -rw-rw-rw- 1 user staff 39 Jun 21 15:37 file1
    -rw-r–r– 1 user staff 35 Jun 21 15:32 file2
    ```
    ```
    $ chmod o-w file1
    $ ls -l
    -rw-rw-r– 1 user staff 39 Jun 21 15:37 file1
    -rw-r–r– 1 user staff 35 Jun 21 15:32 file2
    ```
    * syntax (numeric mode):

    ```chmod [mode] file```
    
    * The mode is a combination of three digits – the first digit indicates the permission for the user, the second digit for the group, and the third digit for others.
    * Each digit is computed by adding the associated permissions. Read permission is ‘4’, write permission is ‘2’ and execute permission is ‘1’.
    * **Example:** Give read/write/execute permission to the user, read/execute permission to the group, and execute permission to others.
    
    ```
    $ ls -l
    -rw-r–r– 1 user staff 39 Jun 21 15:37 file1
    -rw-r–r– 1 user staff 35 Jun 21 15:32 file2
    ```

    ```
    $ chmod 777 file1
    $ ls -l
    -rwxrwxrwx 1 user staff 39 Jun 21 15:37 file1
    -rw-r–r– 1 user staff 35 Jun 21 15:32 file2
    ```
2. chown: change ownership of the file.

    * **description:** Only the owner of the file has the rights to change the file ownership.
    * **syntax:** chown [owner] [file]
    * **Example:** Change owner of file1 to user2 assuming that it is currently owned by the current user
    ```$ chown user2 file1```

3. chgrp: change the group ownership of the file.

    * **description:** Only the owner of the file has the rights to change the file ownership.
    * **syntax:** chgrp [group] [file]
    * **Example:** Change group of file1 to group2 assuming it is currently owned by the current user.
    
    ```$ chgrp group2 file1```

While creating a new file, Unix sets the default file permissions. Unix uses the value stored in a variable called umask to decide the default permissions. The umask value tells Unix which of the three set of permissions need to be disabled.

The flag consists of three octal digits, each representing the permissions mask for the user, the group, and others. The default permissions are determined by subtracting the umask value from ‘777’ for directories and ‘666’ for files. The default value of the umask is ‘022’.

4. umask: change default access permissions

    * **description:** This command is used to set the default file permissions. These permissions are read, write and execute permission for owner, group, and others.
    * **syntax:** umask [mode]
    * The mode is a combination of three digits – the first digit indicates the permission for the user, the second digit for the group, and the third digit for others.
    * Each digit is computed by adding the associated permissions. Read permission is ‘4’, write permission is ‘2’ and execute permission is ‘1’.

    *Example:* Give read/write/execute permission to the user, and no permissions to group or others. i.e. the permission for files will be 600, and for directories will be 700.

    ```$ umask 077```

    * Example: Give read/write/execute permission to the user, read/execute permissions to group or others for directories and read-only permission to group or others for other    files. i.e. the permission for files will be 644, and for directories will be 755.

    ```$ umask 022```

### Conclusion
The permissions in Unix can be for the “user”, “group” and for all the users called, “Other”.

The details like permission flags, link count, owner, group, size, date of last modification, file, etc., can be simply obtained with the command, “ls-l”.