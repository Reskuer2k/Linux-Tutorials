# Chapter 7: Access Control Lists (ACLs)

## 07.1 Introduction to ACLs
ACLs, or Access Control Lists, in Linux are a feature that provides more flexible and granular control over file and directory permissions than the traditional Unix "owner, group, other" system. They allow you to specify access rights for individual users and groups beyond the standard set, enabling you to grant specific read, write, or execute permissions. You manage ACLs using the setfacl command to set them and getfacl to view them.  

### Why Use ACLs?
The traditional permission model in Linux is limited to defining access rights for three entities: 
- The owner (user)
- The group
- Others (everyone else)

ACLs allow you to assign specific permissions to multiple users or groups for the same file or directory, making them useful in more complex environments.

Note. ACL is enabled by default on Red Hat

---

## 07.2 Working with ACLs

Before using `getfacl` or `setfacl` you may need to install the package:

`sudo apt install acl`

### 07.2.1 Viewing ACLs
To view the current ACLs of a file or directory, use the `getfacl` command:

`getfacl filename`

This will display any ACLs associated with the file, in addition to the standard user-group-other permissions.

### 07.2.2 Setting ACLs
To assign ACLs, use the `setfacl` command. The basic syntax is as follows:

`setfacl -m u:username:permissions filename`

- `-m`: Modify ACL
- `u:username`: Assigns permissions to a specific user
- `permissions`: Can be `r` (read), `w` (write), `x` (execute)
- 'filename':Name of the file

#### Example: Granting Permissions
To grant the user `bob` read and write permissions on a file:

`setfacl -m u:bob:rw filename`

### 07.2.3 Removing ACLs
To remove an ACL for a specific user, use:

`setfacl -x u:username filename`

To remove all ACLs from a file or directory:

`setfacl -b filename`

---

## 07.3 Recursive ACLs
ACLs can be applied recursively to directories, allowing the same permissions to be applied to all files and subdirectories.

`setfacl -R -m u:bob:rwx directoryname`

This command grants read, write, and execute permissions to `bob` for all files and subdirectories in the directory.

---

## 07.4 Default ACLs
Default ACLs can be set on directories to ensure that newly created files inherit certain permissions.

`setfacl -m d:u:username:permissions directoryname`

The `d:` prefix indicates that this is a default ACL. Any new files created in this directory will automatically inherit the specified permissions.

#### Example: Setting a Default ACL[linux-sandbox](../../../linux-sandbox)
To ensure that all new files in a directory grant `bob` read and write permissions:

`setfacl -m d:u:bob:rw directoryname`

---

## 07.5 Practical Example of ACL Usage

### Scenario
You have a file named `project.txt` owned by `alice`, and you want to give `bob` read and write permissions without altering group ownership.

#### Step-by-step Example:

1. **View the current ACL of the file**:

    `getfacl project.txt`

2. **Grant `bob` read and write permissions**:

    `setfacl -m u:bob:rw project.txt`

3. **Verify the changes**:

    `getfacl project.txt`

---

## 07.6 Summary
Access Control Lists (ACLs) provide more flexibility in managing permissions for files and directories by allowing you to specify access rights for multiple users and groups. ACLs are particularly useful in environments where multiple users require varying levels of access to the same resources. By mastering ACLs, Linux administrators can efficiently control access while maintaining system security.
