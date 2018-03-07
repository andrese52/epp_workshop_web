---
title: Quick Recap
weight: 2
chapter: false
pre: "<b>2.2 </b>"
---

Let's warm up by reviewing very quickly the commands you learned at the pre-workshop set-up.

**Filesystem** is a term that describes how data in arranged on a computer. It's made out of directories and files. Directory = folder.

**Prompt** $ or % indicates command-line = computer is waiting for your command

Directories and files have an "address", which is called **path**.

**Types of paths:**

1. Absolute path starts with '/' that indicate it starts from the root directory (where it all begins)
2. Relative path is relative to our current working directory (from this point forward)

In the **Cowboy**:

Each user has 2 private directories: /home and /scratch. Nothing backed up!

+ Your /home has 25Gb storage quota. The home directory is also written as **~**.

+ Your /scratch is for large files, space is "unlimited" and shared.

The /opt is a shared directory that contains the applications (aka programs).

When you log in, you are automatically in your /home, which is located in one of the login nodes.

In the login nodes, you only can use unix commands to edit and manipulate files and scripts. If you want to run a job or a script, you must use the batch scheduler.

The batch scheduler receives your job, then puts it in the queue to be executed in a computer node. Once your job finishes, the scheduler sends the results back to your log in folder.

### **Any questions?**
