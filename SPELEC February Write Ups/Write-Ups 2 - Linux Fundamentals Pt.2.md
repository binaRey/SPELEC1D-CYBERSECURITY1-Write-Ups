# **Linux Fundamentals Pt. 2 — TryHackMe Write-Up**

**Author:** Reynan S. Cortado  
 **Platform:** TryHackMe  
 **Topic:** Linux Fundamentals

**Operating Systems Used**

* Ubuntu

* Kali Linux

---

# **Introduction**

This document presents notes and learning insights from **Linux Fundamentals Part 2** on TryHackMe. The module expands on the knowledge introduced in Part 1 by introducing remote machine access, command flags, deeper filesystem interaction, and the fundamentals of Linux permissions.

Part 2 transitions from an **in-browser terminal environment** to interacting with Linux machines through **remote access protocols**, which reflects real-world cybersecurity and system administration workflows.

The exercises emphasize practical skills including:

* Securely connecting to remote machines

* Extending command behavior through flags and switches

* Managing files and directories

* Understanding file permissions and user access

* Identifying important Linux system directories

These skills represent core competencies required in cybersecurity labs and real infrastructure environments.

---

# **Accessing a Linux Machine Using SSH**

The primary concept introduced in this section is **remote machine access** using the Secure Shell protocol.

Secure Shell, commonly abbreviated as **SSH**, is a protocol used for securely connecting to a remote computer over a network.

SSH enables command-line interaction with another machine as if the terminal session were running locally.

---

## **What is SSH?**

SSH is a cryptographic network protocol used to securely access and manage remote systems.

Through encryption, any command entered in the terminal is transformed into encrypted data before traveling across a network. Once the encrypted data reaches the destination machine, it is decrypted and executed.

Key characteristics of SSH include:

* Secure encrypted communication

* Remote command execution

* Authentication through credentials or cryptographic keys

* Wide adoption in system administration and cybersecurity

SSH is a standard method for managing Linux servers across the internet.

---

## **FYI: Why SSH Is Important in Cybersecurity**

In cybersecurity environments, SSH is frequently used to:

* Access remote penetration testing machines

* Manage servers in cloud infrastructure

* Perform system administration tasks

* Securely execute commands across networks

Many cybersecurity platforms and labs rely heavily on SSH connections.

---

# **Deploying the Lab Machines**

The TryHackMe lab environment involves two machines:

1. A **Linux target machine**

2. The **AttackBox**

The AttackBox is a cloud-hosted Linux environment provided by TryHackMe. It functions as a workstation used to interact with other machines in the lab environment.

Both machines communicate through the network using SSH.

---

# **Using SSH to Connect to a Remote Machine**

Connecting to a remote machine requires two essential pieces of information:

1. The IP address of the target machine

2. Valid login credentials for an existing account

The SSH command allows terminal access to the remote machine after successful authentication.

Once connected, any command executed in the terminal runs on the remote machine rather than the local system.

---

## **FYI: Hidden Password Input**

When entering passwords during SSH authentication, the terminal provides no visible characters while typing. This behavior is a security feature designed to prevent password observation.

The password is still being recorded correctly despite the absence of visible feedback.

---

# **Introduction to Flags and Switches**

Many Linux commands accept additional parameters known as **flags** or **switches**.

Flags modify or extend the default behavior of a command.

These arguments usually begin with a hyphen (`-`) or double hyphen (`--`).

Without flags, commands operate using their default functionality.

For example, the `ls` command normally lists files and directories within the current working directory. Certain types of files may remain hidden unless specific flags are used.

---

## **Understanding Command Options**

Flags enable additional functionality such as:

* displaying hidden files

* formatting output differently

* providing additional system information

Each command has its own supported flags.

Learning how to identify available flags significantly expands the usefulness of Linux commands.

---

# **Accessing Command Documentation**

Linux systems provide built-in documentation for commands through **manual pages**, often referred to as **man pages**.

Manual pages contain detailed information about:

* command usage

* supported flags

* syntax examples

* command descriptions

The `man` command allows access to this documentation directly within the terminal.

Manual pages are one of the most valuable learning resources available in Linux environments.

---

## **FYI: Manual Pages Are Essential for Learning Linux**

Manual pages often contain far more information than introductory tutorials. System administrators and security professionals regularly consult man pages when working with unfamiliar commands.

Becoming comfortable reading manual documentation is an important Linux skill.

---

# **Filesystem Interaction Continued**

Linux Fundamentals Part 2 expands filesystem interaction beyond simply viewing files and directories.

Additional commands introduce the ability to:

* create files

* create directories

* move files

* copy files

* delete files

* identify file types

These operations form the core of everyday Linux file management.

---

# **Creating Files and Directories**

Linux allows quick creation of files and directories directly from the command line.

Files can be created as blank placeholders and later populated with data through other tools such as text editors or output redirection.

Directories serve as containers used to organize files within the filesystem hierarchy.

---

## **FYI: Blank File Creation**

Creating a file does not automatically place content inside the file. The file initially exists as an empty container until data is written into it.

Text editors, command redirection, or scripts may later populate the file with content.

---

# **Removing Files and Directories**

Linux also provides commands for removing unwanted files and directories.

Deleting files removes them from the filesystem permanently.

Directories require additional handling because they may contain other files and subdirectories.

Understanding deletion commands is important because incorrect usage may remove critical system data.

---

## **FYI: File Deletion Is Permanent**

Unlike graphical operating systems, many Linux deletion commands do not send files to a recycle bin. Removed files are typically erased immediately.

Careful verification of command arguments is recommended before executing deletion operations.

---

# **Copying and Moving Files**

File management frequently involves copying files or relocating them to different directories.

Linux provides commands that allow:

* duplication of existing files

* relocation of files to new locations

* renaming of files and directories

Moving operations can also function as a method of renaming files.

---

# **Determining File Types**

File extensions often provide hints about file content. However, Linux does not rely solely on file extensions.

Instead, Linux analyzes the **actual structure of the file data** to determine file type.

Commands exist that inspect the binary structure of a file to determine whether it contains text, executable instructions, or another format.

---

## **FYI: File Extensions Are Not Always Reliable**

File extensions can be renamed without changing the file's true content.

For example, a file named `document.txt` might not actually contain text.

This concept is important in cybersecurity, where malicious files may disguise themselves using misleading file extensions.

---

# **Understanding Linux Permissions**

Linux systems implement a structured permission model that controls access to files and directories.

Permissions determine what actions are allowed for different users.

The three primary permission types include:

* **Read** – ability to view file contents

* **Write** – ability to modify file contents

* **Execute** – ability to run a file as a program

Permissions play a major role in system security.

---

# **Users and Groups**

Linux manages access through two identity categories:

* individual **users**

* **groups** of users

A file typically has:

* an owner

* an associated group

* permission settings that apply to other users

This structure allows administrators to create flexible access control systems.

---

## **FYI: Permission Systems Protect Linux Systems**

Permissions prevent unauthorized users from modifying critical system files.

For example:

* application files may only allow reading

* configuration files may allow editing only by administrators

* executable files may require special permissions to run

This layered permission system helps protect Linux systems from accidental or malicious changes.

---

# **Switching Between Users**

Linux systems allow switching between user accounts.

This process requires knowledge of the target user's credentials.

Switching users allows temporary access to files or permissions belonging to another account.

This capability is frequently used in system administration and security testing.

---

## **FYI: User Switching in Security Testing**

In penetration testing environments, switching users may reveal additional access privileges.

If credentials are discovered during testing, switching accounts may lead to elevated permissions or additional system access.

---

# **Important Linux System Directories**

Linux systems contain several critical root directories that store configuration data, logs, and temporary files.

Understanding these directories helps analysts navigate Linux systems more effectively.

---

## **/etc Directory**

The `/etc` directory stores configuration files used by the operating system and installed applications.

Common contents include:

* user account configuration files

* system service configurations

* administrative permission lists

Many core system settings are stored within this directory.

---

## **/var Directory**

The `/var` directory stores **variable data** generated by services and applications.

Typical contents include:

* system logs

* application logs

* cached data

* database storage

Logs stored in this directory are particularly valuable when analyzing system behavior or investigating incidents.

---

## **/root Directory**

The `/root` directory functions as the **home directory of the root user**, which is the administrative account on Linux systems.

Unlike regular users whose home directories exist under `/home`, the root account maintains a separate directory.

---

## **/tmp Directory**

The `/tmp` directory is used for temporary storage.

Files stored here are often removed automatically after system reboots.

Many programs temporarily store data in this directory while executing tasks.

---

## **FYI: /tmp in Security Testing**

The `/tmp` directory is commonly used in penetration testing because most Linux systems allow any user to write files there.

Scripts, tools, and temporary payloads are often stored in this directory during testing.

---

# **Key Beginner Insights**

### **Remote Access Is a Core Linux Skill**

Managing remote machines is a standard practice in Linux environments. SSH provides the primary method for interacting with remote servers securely.

---

### **Flags Significantly Expand Command Capabilities**

Basic Linux commands become far more powerful when combined with flags. Learning command options greatly improves efficiency.

---

### **Filesystem Management Is Essential**

Creating, copying, moving, and deleting files are fundamental skills required in system administration and cybersecurity labs.

---

### **Permissions Control System Security**

Linux permission systems regulate who can read, modify, or execute files. Understanding permissions helps prevent unauthorized access.

---

### **System Directories Contain Valuable Information**

Directories such as `/etc` and `/var` often contain configuration files and logs that provide insight into how a system operates.

---

# **Personal Learning Notes**

Important observations from this module include:

* Remote access using SSH is essential for interacting with Linux servers

* Manual pages provide extensive documentation for most commands

* File permissions form a major component of Linux system security

* Understanding the filesystem hierarchy improves navigation and system analysis

Regular practice with commands and directory navigation strengthens familiarity with Linux environments.

---

# **Next Steps**

The next module in the series continues expanding Linux knowledge with additional command usage, system utilities, and scripting concepts.

Further topics commonly explored include:

* advanced command pipelines

* text processing tools

* environment variables

* process management

---

# **Final Thoughts**

Linux Fundamentals Part 2 deepens foundational Linux knowledge by introducing remote access, permission systems, and extended command functionality.

These concepts are widely used in cybersecurity environments, especially when interacting with remote servers and analyzing Linux-based systems.

Mastery of these skills establishes a strong foundation for advanced Linux usage and penetration testing activities.

