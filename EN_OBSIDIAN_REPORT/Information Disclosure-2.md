Vulnerability #14: Information Disclosure 

 

Flag : {SECRET_LOG_12PIERRE34} 

Sévérité : High 

Location: log_system_event command (sub_40269e)  

Description:  

 

The logging system contains a hidden debug command. By entering the keyword "leak" in the logs module, the program writes a secret administration key to a persistent file on disk (Data/system.log).  

 

Exploitation analysis:  

  

1. Retro engineering with Binary Ninja shows a sensitive function.  

  

2. We detect sentences that correspond to a binary command  

  

known. This one corresponds to the "log_system_event" function  

Prerequisites: The existence of a writable Data/ directory.  

Trigger: The call to function sub_40269e via the main menu.  

3. The function contains insecure parts, including a comparison. If the user enters exactly the word "leak," it sends  

a piece of sensitive information to 'Data/system.log'  

 

4. All that’s left for us to do is view the contents of the file:  

cat Data/system.log  

EVENT: leak 

SECRET_KEY_LEAKED: {SECRET_LOG_12PIERRE34}  

 

Impact:  

An attacker with even restricted access to the filesystem can collect high-level secrets by monitoring log files, thereby bypassing console display protections.  

 

Resolution:  

Delete all test functions, even if they are not directly referenced in the help.  

A user warns using a decompiling tool, or simply a typing error could lead to sensitive information.  

Set an administrator privilege of the type if (admin == 1) -> fprintf  

instead of a single file.  

fprintf is also a bad idea, because this function stores in a file for long periods of time; the or a simple printf displays in the console without data persistence.  

 

 

 

 

 

 

 

Vulnerabily 15 Hardcoded credentials. 

 