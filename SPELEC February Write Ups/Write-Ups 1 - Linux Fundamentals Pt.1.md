# **Linux Fundamentals Pt. 1 — TryHackMe Write-Up**

**Author:** Reynan S. Cortado  
 **Platform:** TryHackMe  
 **Topic:** Linux Fundamentals

**Operating Systems Used**

* Ubuntu

* Kali Linux

---

# **Introduction**

**This document presents notes and insights from Linux Fundamentals Part 1 on TryHackMe. The module introduces the basics of the Linux operating system, terminal interaction, and filesystem navigation.**

**Linux knowledge is fundamental for fields such as:**

* **Cybersecurity**

* **System administration**

* **Networking**

* **Software development**

**The virtual machine provided in this room runs Ubuntu, allowing safe practice of Linux commands directly from the browser.**

---

# **Background on Linux**

**Linux is an open-source operating system kernel created in 1991 by Linus Torvalds.**

**The open-source nature of Linux allows developers and organizations to modify and distribute the software freely. This flexibility has contributed to its widespread adoption across servers, cloud systems, and cybersecurity environments.**

## **Key Characteristics of Linux**

**Linux is widely used due to the following qualities:**

*  **Lightweight system performance**

* **Strong security architecture**

* **High flexibility and customization**

* **Open-source licensing**

* **Stability for servers and infrastructure**

**Linux powers a large portion of global technology infrastructure, including:**

* **Web servers**

* **Cloud platforms**

* **Embedded systems**

* **Cybersecurity labs**

---

# **Common Linux Distributions**

**A Linux distribution (distro) is a packaged version of Linux containing system utilities, software, and a package manager.**

**Popular distributions include:**

* **Kali Linux – widely used for penetration testing and ethical hacking**

* **Ubuntu – beginner-friendly and commonly used in servers**

* **Fedora – developer-focused distribution with newer technologies**

* **Arch Linux – minimalist distribution with extensive customization**

---

# **Interacting With a Linux Machine (Browser-Based)**

**The TryHackMe platform provides an in-browser virtual machine environment.**

**Upon deployment, the system provides:**

* **A temporary IP address**

* **A time limit before the machine shuts down automatically**

**This setup allows Linux command practice without installing software locally.**

**The distribution used in this environment is Ubuntu.**

---

# **Running First Linux Commands**

**The introductory commands presented in this module are:**

* **`echo`**

* **`whoami`**

**These commands demonstrate how the terminal interacts with the operating system.**

---

## **echo — Display Text**

**The `echo` command prints text directly to the terminal.**

**Example:**

**echo "Hello World"**

**Output:**

**Hello World**

### **Common Uses**

**The `echo` command is often used for:**

* **Displaying messages in scripts**

* **Testing shell commands**

* **Redirecting text into files**

**Example:**

**echo "Linux Fundamentals"**

---

## **whoami — Identify Current User**

**The `whoami` command displays the username of the current session.**

**Example:**

**whoami**

**Example output:**

**ubuntu**

**Understanding the active user account is important because different accounts possess different system permissions.**

---

# **Interacting With the Linux Filesystem**

**The Linux filesystem organizes data using directories (folders) and files.**

**The main commands introduced include:**

* **`ls`**

* **`cd`**

* **`cat`**

* **`pwd`**

**These commands provide the foundation for navigating a Linux system.**

---

## **ls — List Directory Contents**

**The `ls` command lists files and directories located in the current directory.**

**Example:**

**ls**

**Example output:**

**Documents  Downloads  notes.txt**

### **Useful Options**

**ls \-l**

**Displays detailed file information including permissions and file size.**

**ls \-a**

**Displays hidden files.**

---

## **cd — Change Directory**

**The `cd` command changes the current directory.**

**Example:**

**cd folder1**

**The current location becomes folder1.**

**Returning to the previous directory:**

**cd ..**

**Directories function similarly to folders in graphical operating systems.**

---

## **cat — Display File Content**

**The `cat` command prints the content of a file.**

**Example:**

**cat file1.txt**

**Example output:**

**This is the content of file1.txt**

### **Typical Uses**

* **Viewing text files**

* **Reading configuration files**

* **Inspecting system logs**

---

## **pwd — Print Working Directory**

**The `pwd` command displays the current directory path.**

**Example:**

**pwd**

**Example output:**

**/home/ubuntu**

**This command is useful when navigating complex directory structures.**

---

# **Searching for Files**

**Large systems often contain thousands of files. Linux provides powerful tools to locate files and analyze file content.**

**Key commands include:**

* **`find`**

* **`grep`**

* **`wc`**

---

## **find — Search Files and Directories**

**The `find` command searches for files based on specified conditions.**

### **Search directories**

**find \-type d**

### **Search files**

**find \-type f**

### **Search a specific file**

**find \-type f \-name note.txt**

### **Search by file extension**

**find \-name \*.txt**

**This command returns all `.txt` files within the directory tree.**

---

## **grep — Search Inside Files**

**The `grep` command searches text patterns within files.**

**Example:**

**grep "hello" file.txt**

**The command returns lines containing the specified keyword.**

**Security analysts commonly use `grep` to inspect:**

* **system logs**

* **configuration files**

* **error messages**

---

## **wc — Count File Statistics**

**The `wc` command counts the number of lines, words, and characters in a file.**

**Example:**

**wc file.txt**

**Example output:**

**10 50 200 file.txt**

**Meaning:**

* **10 lines**

* **50 words**

* **200 characters**

---

# **Shell Operators**

**Shell operators allow commands to be combined or redirected.**

**Operators introduced in this module include:**

* **`&`**

* **`&&`**

* **`>`**

* **`>>`**

---

## **& — Run Processes in Background**

**The `&` operator allows commands to execute in the background while the terminal remains usable.**

**Example:**

**cp largefile.txt backup.txt &**

**The file copy operation continues in the background.**

---

## **&& — Command Chaining**

**The `&&` operator connects multiple commands.**

**The second command executes only if the first command completes successfully.**

**Example:**

**cd folder4 && cat note.txt**

---

## **\> — Output Redirection**

**The `>` operator redirects command output into a file.**

**Example:**

**echo "Hello" \> file.txt**

**This command creates file.txt and stores the text inside.**

**Note: existing content in the file will be overwritten.**

---

## **\>\> — Append Output**

**The `>>` operator appends output to the end of an existing file.**

**Example:**

**echo "Second line" \>\> file.txt**

**File content becomes:**

**Hello**

**Second line**

---

# **Key Beginner Insights**

### **1\. Linux Relies Heavily on the Terminal**

**Command-line interaction is a core part of Linux system management and cybersecurity work.**

---

### **2\. The Filesystem Uses a Hierarchical Structure**

**Linux organizes directories in a tree structure:**

**/**

**├── home**

**├── etc**

**├── var**

**Understanding this structure is essential for navigating systems efficiently.**

---

### **3\. Small Commands Can Be Extremely Powerful**

**Simple commands such as `find` and `grep` become powerful when combined.**

**Example:**

**find . \-name \*.txt | grep password**

**This combination can quickly identify files containing sensitive information.**

---

# **Personal Learning Notes**

**Important observations from this module:**

* **Linux commands follow consistent patterns and syntax**

* **A small set of commands enables efficient system navigation**

* **Regular practice improves command-line familiarity**

**Mastering these fundamentals forms the foundation for advanced Linux usage.**

---

# **Next Topics**

**The following modules typically expand into:**

* **File permissions**

* **User management**

* **Process monitoring**

* **Advanced command usage**

* **Bash scripting**

**These topics are essential for cybersecurity roles such as penetration testing and system administration.**

---

# **Final Thoughts**

**Linux is a fundamental technology within cybersecurity environments. Many security tools and infrastructure platforms rely on Linux systems.**

**Developing familiarity with basic commands and filesystem navigation significantly improves efficiency when working in technical environments.**

