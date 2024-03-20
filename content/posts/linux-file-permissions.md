+++
title = "File permissions in Linux"
author = ["tcluri"]
date = 2022-07-13T12:45:00+05:30
tags = ["linux"]
draft = false
+++

## What are file permissions? {#what-are-file-permissions}

In Linux, file permissions, attributes, and ownership control the access level that
the system processes and users have to files. This ensures that only authorized users
and processes can access specific files and directories.

To find out what permissions exist for files &amp; folders in the current directory,
do `ls -la` after the user prompt in the terminal. It is represented by the first
ten characters that shows for each file/folder present in the directory.

Example file permissions in current directory

```nil
 user$ ls -la
 -rw-r--r-- 1 user user 2203 Jul 23 23:26 foo.py
```


## Permisssion types {#permisssion-types}

There are three permission types:

1.  read(r)
2.  write(w)
3.  execute(x)


## User-based permission groups {#user-based-permission-groups}

The first ten characters at the beginning of the output in the terminal
can be broken down into the following

1.  file type - represented by the zeroth character.
2.  owner - represented in the first 3 characters.
    The Owner permissions apply only to the owner of the file or
    directory, and will not impact the actions of other users.
3.  group - represented in the middle 3 characters.
    The Group permissions apply only to the group that has been
    assigned to the file or directory, and will not affect the
    actions of other users.
4.  all users - represented in the last 3 characters.
    The All Users permissions apply to all other users on the system,
    and this is the permission group that you want to watch the most.

We can say that the owner of the file (in this case, it's `user`) foo.py
has read (r) and write (w) permissions, and the group and the rest of
users have only read (r) permissions.


## Changing file permissions with chmod {#changing-file-permissions-with-chmod}

By using `chmod` we can change the file permissions to let a user-based
permission group have specific access(read/write/execute) to the file.

The `chmod` command usage takes the following arguments

```nil
  chmod  <groups to assign the permissions><permissions to assign/remove> <file/folder names>
```

Groups to assign the permissions can be specified using the following flags

1.  u - owner
2.  g - group
3.  o - others
4.  a - all users. For all users, it can also be left blank

Let us look at few examples of `chmod`


### Example of permission change to a file {#example-of-permission-change-to-a-file}

Adding group's write permission to the file foo.py

```nil
 user$ ls -la
 -rwxr-xr-x  1 user user 2203 Jul 23 23:26 foo.py
 user$ chmod g+w foo.py
 user$ ls -la
 -rwxrwxr-x  1 user user 2203 Jul 23 23:26 foo.py
```


### Another example of permission change {#another-example-of-permission-change}

Removing group's and others' execute permission to foo.py

```nil
 user$ ls -la
 -rwxrwxr-x 1 user user 2203 Jul 23 23:26 foo.py
 user$ chmod go-x foo.py
 user$ ls -la
 -rwxrw-r-- 1 user user 2203 Jul 23 23:26 foo.py
```


### Setting chmod file permissions using Binary References {#setting-chmod-file-permissions-using-binary-references}

Another method in which you can modify the permissions of a file
using `chmod` is via binary references which can be described as
follows.

The whole string stating the permissions (rwxrwxrwx) is substituted
by 3 numbers. The first number represents the Owner permission(u); the
second represents the Group permissions(g); and the last number represents
the permissions for all other users(o).

The permission types have assigned values and the sum of the
combinations is taken as permissions for each user-based permission group.

-   r = 4 # read
-   w = 2 # write
-   x = 1 # execute


#### Example for permission change using Binary Reference {#example-for-permission-change-using-binary-reference}

Assign to the foo.py file the following permissions:

-   Owner: Read, Write and Execute
-   Group: Read and Execute
-   All others: Read and Execute

_Calculating the user-based permission groups values:_

Owner: r+w+x      = 4+2+1 = 7 <br />
Group: r+x        = 4+0+1 = 5 <br />
All others: r+x   = 4+0+1 = 5

```nil
 user$ ls -la
 -rwxrw-r-- 1 user user 2257 Jul 12 01:23 foo.py
 user$ chmod 755 foo.py
 user$ ls -la
 -rwxr-xr-x 1 user user 2257 Jul 12 01:23 foo.py
```
