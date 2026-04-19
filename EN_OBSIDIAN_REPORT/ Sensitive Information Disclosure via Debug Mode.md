# Vulnerability #10: Sensitive Information Disclosure via Debug Mode 

## Flag: {SECRET DIAGNOSTIC KEY}  

Severity: High  

Type: Information Disclosure & Insecure Debug Configuration  

 

Description:  

The run_diagnostic command offers several operating modes: normal, debug and advanced. Debug mode was not disabled or password protected in the production version of the binary. Enabling this mode forces the program to display a diagnostic secret key that should not be accessible by end users.  

Technical Analysis:  

When calling the function associated with run_diagnostic, the program performs a string comparison (strcmp) on the user input.  

If the entry is "debug", the execution flow takes a specific branch that directly displays the contents of a buffer containing the {SECRET DIAGNOSTIC KEY}.  

 

PoC:  

Launch the binary: . /obsidian.  

Run command: run_diagnostic.  

At the "Enter diagnostic mode (normal/debug/advanced):" prompt, enter: debug.  

Revelation of sensitive data: Diagnostic result: {SECRET DIAGNOSTIC KEY}.  

 

Impact:  

Retrieving a diagnostic key can allow an attacker to authenticate other maintenance interfaces, decipher logs, or manipulate reactor sensors by posing as a technical member.  

 

Patch:  

Use build guidelines (#ifdef DEBUG) to ensure that code related to debug mode is not included in the final binary.  

If a diagnostic mode is required in production, it must be protected by an authentication method  

Do not store keys in clear text in diagnostic messages. 

 

 

 

 