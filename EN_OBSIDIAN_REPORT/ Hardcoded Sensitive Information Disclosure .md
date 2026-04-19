# Vulnerability: Hardcoded Sensitive Information Disclosure 

## Flag: {Sensitive Data} 

Severity: Critical 


Type: Information Disclosure & Hardcoded credentials 

Location: obsidian binary, function sub_401cdc (Command: check_cooling_pressure) 

Discovered in: Reverse Engineering (Blackbox) 

 

Description: 

 

The check_cooling_pressure command contains a major information leak. During its execution, the program initializes a local buffer with a secret string via the __builtin_strncpy instruction at the address 0x00401d0a. This data, probably intended for debugging, is then sent to the STDOUT output via a system call at the address 0x00401db7.  

 

PoC:  

    Execute the binary . /obsidian.  

    Run check_cooling_pressure.  

    The system displays: Sensitive Info: {Sensitive Data}.  

  

Impact:  

 

Information Disclosure. Exposing hard-coded secrets allows an attacker to pivot to other secure sections of the Obsidian system or gain administrative privileges. 

Patch:  

 

Delete the var_38 variable and the call to strncpy containing the flag.  

Disable all "Sensitive Info" or "Debug" type displays in production versions.  

Never store sensitive hard-coded data in functions; use environment variables or digital safes if the data is necessary for operation.  
![alt text](<../Images/SENSITIVE DATA.png>)