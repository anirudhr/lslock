RUNNING INSTRUCTIONS
===============
lslock:
* To run:
  * `python3 lslock DIRECTORY` OR
  * `python3 lslock` to run on the pwd OR
  * `chmod u+x lslock; ./lslock DIRECTORY` OR
  * `chmod u+x lslock; ./lslock` to run with the pwd as input
DIRECTORY can be a full or a relative (to pwd) Linux system path 

lslock-test:
* Needs to be in the same directory as lslock.
* Needs to have rwx permissions to access / create a directory in /tmp and create and modify files within that directory.
* To run:
  * `python3 lslock-test` OR
  * `chmod u+x lslock-test; ./lslock-test`.
  
INTRODUCTION
===============
The flock syscall (and command line program) provides an easy and safe way to coordinate multiple programs on a Linux system: a lock is taken against a file and if any other process attempts to take this lock, it blocks. The lock is associated with a file descriptor, so within the program holding the lock, it can be locked multiple times idempotently; and child processes can even inherit the lock.

When a program uses multiple locks (for example, to allow work stealing for multiple instances of a job), it can be helpful to get a listing of which paths are locked by which processes.

One way to get this listing is by leveraging the synthetic file /proc/locks, which lists locks, one per line, with their inode, PID and various flags. Assuming the application stores all temporary state under one directory, with one subdirectory per job, then one can list all the *.lock files under this directory with their inodes and pair them up with the values in /proc/locks to a get a listing of two columns, PID and path, for each locked file.

CHALLENGE
===============
Write a program, lslock, that accepts a directory argument and prints the PIDs and paths of all files locked beneath it. Write a test program, lslock-test, that launches a few background processes to lock files in the directory /tmp/lslock-test and verifies that your lslock does indeed find all the locks. If more than one process holds a lock on the same file -- for example, a parent and one of its children -- it is okay to list the lock multiple times.
You may use the language of your choice. Please include instructions for building and running both lslock and lslock-test.
