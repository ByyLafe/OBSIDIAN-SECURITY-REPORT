# Vulnerability #9: Race Condition via Insecure Temporary File 

## Flag : {ACCESS_GRANTED} 

Severity: Critical 

Type: Race Condition / Information Disclosure 

Location: turbine_remote_access.c, function turbine_remote_access : 0x00401669 

Discovered in: obsidian library 

 

Description: 

The turbine_remote_access function generates a temporary file to store an access key. Three major design errors make this procedure vulnerable:  

  

1. The program explicitly displays the path of the secret file in the terminal (Temporary file created: %s).  

2. The program stops deliberately for 5 seconds (sleep(5)) after writing the secret and before deleting the file.  

3. Call to unlink (delete) occurs only after this timeout, leaving a time window sufficient for unauthorized reading.  

 

PoC:  

 

1. Launch the turbine_remote_access command in the main program.  

2. Note the name of the generated file (e.g., Data/remote_access8VD0G4).  

3. During the 5-second timeout, open a second terminal and execute:  

cat Data/remote_access8VD0G4  

4. The secret is then displayed: {ACCESS_GRANTED}.  

  

(It would also have been possible to develop a script allowing you to extract the file very quickly once created, if the sleep time was too short to do it by hand)  

  

Impact:  

Privilege Escalation / Information Disclosure.  

A local user can retrieve credentials or access keys that are supposed to be ephemeral and private, allowing the remote access to the turbine to be compromised.  

 

Patch:  

The use of printf here is not relevant; if it’s a debug-related function, it should have been removed before pushing the program into production.  

Remove the 5-second delay allowing easy interception of data present in the file.  

If the file is to be stored somewhere, it should, if possible, be hashed, and if it cannot be hashed because another part of the program uses it, it should be stored in RAM, not on a disk. 