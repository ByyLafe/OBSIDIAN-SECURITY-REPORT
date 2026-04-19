# Vulnerability: Privilege Escalation 

## Flag: {ADMIN4242}  

Severity: Critical  

Type: Hardcoded Credentials / Improper Access Control  

Location: obsidian binary, function sub_401bd5 (Command: unlock_secret_mode)  

Discovered in: Reverse Engineering (Blackbox)  

  

Description:  

Analyzing the binary with reverse engineering tools revealed the existence of a command not listed in the standard help menu: unlock_secret_mode. This function (sub_401bd5) is a backdoor probably intended for developers in case they forget their password.  

When decompiling the code, it is very clear that this function only serves to access administrative privileges through it.  

 

PoC:  

    Execute the binary . /obsidian.  

    Manually enter the hidden command: unlock_secret_mode.  

    The flag is revealed: {ADMIN4242}.  

 

Impact:  

This flaw allows any advanced user, knowing that they are using a reverse engineering tool (here Ghidra and Binary Ninja), to obtain administrator access, if they have previously obtained administrative rights.  

 

Patch:  

 

Removing hard-coded credentials: Never store passwords in plain text in source code or binary. Use a secure hash system (like SHA-256 with salt).  

Delete "secret mode" or debug features in production. All administrative orders must be documented, audited, and protected by robust authentication (ideally linked to a secure directory service).  

Make sure that the elevation of privileges requires validation by an authority (for example, a remote server, via a dedicated IT service) and not just a simple local text comparison. 

 

 

 