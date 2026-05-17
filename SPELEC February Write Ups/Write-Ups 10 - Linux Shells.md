# **Linux Shells — TryHackMe Write-Up**

**Author:** Reynan S. Cortado  
 **Platform:** TryHackMe  
 **Topic:** Command Line

---

# **Introduction**

Linux systems can be controlled either through a **Graphical User Interface (GUI)** or a **Command Line Interface (CLI)**. While the GUI is convenient, the **CLI is more powerful, efficient, and widely used in cybersecurity and system administration**.

The **shell** acts as the **intermediary between the user and the operating system**, allowing users to execute commands that interact with system resources.

In cybersecurity labs and penetration testing environments, mastering the **Linux shell** is essential because many tasks such as **enumeration, file analysis, automation, and scripting** rely heavily on command-line tools.

---

# **Tools & Operating Systems**

### **Tools Used**

* **Linux Terminal**

* **Nano** (text editor)

* **grep** (pattern search tool)

* **chmod** (permission modification tool)

### **Operating Systems**

* **Linux (Ubuntu-based environment)** used in the TryHackMe virtual machine.

---

### **⚡ Tip**

Most cybersecurity labs provide a **remote Linux machine**. Knowing how to navigate the CLI efficiently will save you **significant time during CTF challenges and penetration testing tasks**.

---

### **Quick Summary**

* The **shell** allows users to interact with the OS via commands.

* Linux CLI is **faster and more efficient** than GUI for advanced tasks.

* Bash is the **most common default shell**.

---

# **Remote Access / SSH**

In cybersecurity labs, we often connect to remote machines using **SSH (Secure Shell)**.

**SSH** is a cryptographic network protocol used to securely connect to remote systems over a network.

### **Why SSH Is Important**

* Provides **secure encrypted communication**

* Allows **remote system administration**

* Widely used in **penetration testing labs and cloud servers**

### **Example SSH Connection**

ssh username@target-ip

Example:

ssh user@10.10.10.10

If a specific port is required:

ssh user@10.10.10.10 \-p 2222  
---

### **⚡ Tip**

In most CTF labs:

* Username is often **user**

* Password is provided in the task instructions.

---

### **Quick Summary**

* **SSH** enables secure remote access.

* Syntax:

ssh username@ip

* Commonly used to connect to **lab machines or servers**.

---

# **Command Flags & Documentation**

## **What Are Flags?**

**Flags (or switches)** modify how commands behave.

Example:

ls \-l

The `-l` flag displays the **long listing format**.

---

## **Viewing Documentation**

Linux commands include built-in documentation accessible through **man pages**.

man ls

Example usage:

man grep

To exit the manual page:

Press q  
---

## **Common Command Examples**

ls  
pwd  
cd Desktop  
cat file.txt  
grep keyword file.txt  
---

## **Commands & Flags Table**

| Command | Common Flags | Description |
| ----- | ----- | ----- |
| `ls` | `-l`, `-a`, `-la` | Lists directory contents |
| `pwd` | None | Displays current directory |
| `cd` | None | Changes directory |
| `cat` | None | Displays file contents |
| `grep` | `-i`, `-r`, `-n` | Searches patterns in files |
| `chmod` | `+x` | Changes file permissions |
| `history` | None | Displays command history |

---

### **⚡ Tip**

Use **Tab Completion** to auto-complete commands and paths.

Example:

cd Doc\<TAB\>  
---

### **Quick Summary**

* Flags modify command behavior.

* `man` provides command documentation.

* Key commands include **ls, cd, cat, grep**.

---

# **Filesystem Management**

Linux organizes files using a **hierarchical directory structure**.

## **Display Current Directory**

pwd

Example output:

/home/user  
---

## **List Directory Contents**

ls

Example output:

Desktop Documents Downloads Music Pictures  
---

## **Navigate Directories**

cd Desktop

Return to previous directory:

cd ..  
---

## **Read File Contents**

cat filename.txt

Example output:

this is a sample file  
this is the second line  
---

## **Searching Inside Files**

grep THM dictionary.txt

Output:

The flag is THM  
---

### **⚠️ FYI**

**File deletion in Linux is permanent** (especially in CLI). There is **no recycle bin** unless a special tool is used.

---

### **Quick Summary**

* `pwd` shows current location.

* `ls` lists directory contents.

* `cd` changes directories.

* `cat` reads file contents.

* `grep` searches inside files.

---

# **Permissions**

Linux uses a **permission system** to control access to files and directories.

### **Three Basic Permissions**

| Permission | Meaning |
| ----- | ----- |
| **Read (r)** | View file contents |
| **Write (w)** | Modify the file |
| **Execute (x)** | Run the file as a program |

Permissions apply to:

* **User (Owner)**

* **Group**

* **Others**

---

## **Making Scripts Executable**

chmod \+x script.sh

Run the script:

./script.sh  
---

### **⚡ Tip**

Always make scripts executable before running them.

---

### **Quick Summary**

* Linux permissions control **file access**.

* `chmod` modifies permissions.

* `+x` makes a script executable.

---

# **Shell Types**

Linux supports multiple shell environments.

## **Check Current Shell**

echo $SHELL

Example output:

/bin/bash  
---

## **List Available Shells**

cat /etc/shells

Example:

/bin/bash  
/bin/zsh  
/bin/dash  
/bin/sh  
---

## **Common Shells**

| Shell | Description |
| ----- | ----- |
| **Bash** | Default shell in most Linux systems |
| **Fish** | Beginner-friendly with syntax highlighting |
| **Zsh** | Highly customizable shell |

---

## **Switch Shell**

zsh

Change default shell permanently:

chsh \-s /usr/bin/zsh  
---

### **Quick Summary**

* **Bash** is the most common Linux shell.

* `echo $SHELL` shows current shell.

* `/etc/shells` lists installed shells.

---

# **Shell Scripting**

Shell scripts automate repetitive tasks.

Scripts typically include:

* **Shebang**

* **Variables**

* **Loops**

* **Conditional Statements**

---

## **Shebang**

Defines the interpreter used for the script.

\#\!/bin/bash  
---

## **Variables Example**

\#\!/bin/bash

echo "Hey, what's your name?"  
read name  
echo "Welcome, $name"

Run the script:

chmod \+x variable\_script.sh  
./variable\_script.sh  
---

## **Loops Example**

\#\!/bin/bash

for i in {1..10}  
do  
echo $i  
done

Output:

1  
2  
3  
...  
10  
---

## **Conditional Statements**

\#\!/bin/bash

echo "Please enter your name:"  
read name

if \[ "$name" \= "Stewart" \]; then  
echo "Welcome Stewart\! Here is the secret: THM\_Script"  
else  
echo "Access denied"  
fi  
---

## **Comments**

\# This is a comment

Used to explain code for readability.

---

### **Quick Summary**

* Shell scripts automate tasks.

* Key components:

  * Shebang

  * Variables

  * Loops

  * Conditional statements

  * Comments

---

# **Important System Directories**

Understanding key Linux directories is important for **system administration and cybersecurity investigations**.

| Directory | Purpose |
| ----- | ----- |
| **/etc** | Stores system configuration files |
| **/var** | Stores logs and variable data |
| **/root** | Home directory of the root user |
| **/tmp** | Temporary files |

---

### **Why These Matter in Cybersecurity**

* **/var/log** → contains **system logs useful for forensic analysis**

* **/etc** → contains **password and configuration files**

* **/root** → sensitive admin data

* **/tmp** → often used for **temporary malicious scripts**

---

### **Quick Summary**

* `/etc` stores configuration files.

* `/var/log` contains important system logs.

* `/root` belongs to the administrator.

* `/tmp` stores temporary files.

---

# **Key Beginner Insights**

* The **shell is the interface between the user and OS**.

* **Bash** is the default Linux shell.

* Important commands include:

pwd  
ls  
cd  
cat  
grep  
chmod  
history

* Shell scripts automate tasks.

* Cybersecurity tasks rely heavily on **CLI tools and scripting**.

---

# **Personal Learning Notes**

* Understanding the **difference between GUI and CLI** helps appreciate the power of the Linux shell.

* **grep is extremely powerful** for searching large log files.

* Shell scripting allows automation of repetitive tasks like **log searching and system monitoring**.

* Permissions (`chmod`) are critical when executing scripts.

---

# **Next Steps / Final Thoughts**

To continue mastering Linux for cybersecurity, consider studying:

### **Recommended Topics**

* **Linux File Permissions Deep Dive**

* **Process Management (`ps`, `top`, `kill`)**

* **Networking Commands (`netstat`, `ss`, `ip`)**

* **Linux Privilege Escalation**

* **Bash Scripting Automation**

Platforms like **TryHackMe** and **Hack The Box** provide excellent hands-on labs to practice these skills.

---

### **Final Thoughts**

Mastering the **Linux command line and shell scripting** is one of the **most important foundational skills in cybersecurity**. Once you are comfortable with these basics, you will be able to navigate systems faster, automate tasks, and analyze environments effectively during penetration tests and CTF challenges.

