# Vulnerability #2: Information Disclosure 

## Flag : {The secret stone is here !} 

Severity: Critical  

Type: Logic Flaw / Information Disclosure / Buffer Overflow  

Location: obsidian binary, function sub_402541 (Command: load_fuel_rods)  

Discovered in: Reverse Engineering (Blackbox)  

 

Description:  

The load_fuel_rods function has an inconsistency in its digital terminal tests. Although the interface advertises a 10 fuel bar limit, entering the value "10" diverts the execution flow to a secure code branch. In addition, improper use of strncpy prevents the correct termination of the character string in memory, forcing the display of a "Secret Key" and causing a memory leak of the stack.  

 

Technical Analysis:  

The normal loading branch runs only for values between 1 and 9. The value 10 is captured by the result_1 > 9 test at address 0x00402610.  

The flag is copied without an ending null byte (\0). The comparison function therefore systematically fails, and the display function (probably printf) continues to read the memory beyond the flag, displaying residues contained in the memory.  

 

PoC:  

Launch . /obsidian.  

Run the command load_fuel_rods.  

At the prompt "Enter the number of fuel rods to load (max 10):", enter 10.  

The system reveals sensitive data: Secret Key: {The secret stone is here! }=p10.  

Impact:  

Information Disclosure. An attacker can extract hardcoded secret keys and gain visibility into the content in memory, which could help build a more complex operation.  

Patch:  

Align the conditions so that the value 10 is processed in the normal loading loop: if (result_1 > = 1 && result_1 <= 10).  

Use snprintf or manually check for a null-terminator: var_c8[size of(var_c8) - 1] = '\0'.  

Delete the code branch revealing the passphrase before the final build.