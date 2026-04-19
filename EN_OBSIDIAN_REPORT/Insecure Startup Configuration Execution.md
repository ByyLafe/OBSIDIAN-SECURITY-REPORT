# Insecure Startup Configuration Execution

## Flag : {Correct password! Welcome, admin.}

Severity: High

Type: Arbitrary Command Execution

Location: 00402f9d

Discovered in: Black-box

Description:

The binary obsidian automatically searches for a hidden configuration file named .obsidianrc in the working directory upon execution. If found, the binary passes the content of this file to a shell-level execution environment (observed via execve calls in strace). There is no validation or sanitization of the commands contained within this file, allowing any user with write access to the directory to execute arbitrary scripts or system commands with the privileges of the obsidian process.

Proof of Concept:

Create a malicious .obsidianrc file that performs an unauthorized memory dump of the binary's data section to exfiltrate secrets: 

echo 'exec python3 -c "print(open(\"obsidian\",\"rb\").read()[726376:726426])"' > .obsidianrc 
./obsidian 

Impact: 

Privilege Escalation and Information Disclosure. An attacker can leverage this to bypass internal security checks, dump memory contents, or maintain persistence on the system. 
