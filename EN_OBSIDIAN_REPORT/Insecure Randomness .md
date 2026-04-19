Vulnerability #12 : Insecure Randomness 

 

Severity: High  

Flag: {MELTDOWN1234} 

Type: Insecure Randomness  

Description:  

The nuclear fusion simulation function (simulate_meltdown) contains a vulnerability; it manipulates sensitive data (here a flag) in memory and displays it only via a random draw (rand type function) without any verification of rights.  

Technical Analysis:  

The binary generates a pseudo-random number between 0 and 99.  

int32_t rax_1 = sub_407100() s% 0x64  

Corresponds to a modulo 100, so it will generate a number between 0 and 100.  

A nested conditional structure checks if this value is less than or equal to 4. If the condition is met, the program executes a "critical error" code branch that displays {MELTDOWN1234} via the standard output function.  

PoC:  

Due to the probability of success set at 5%, it was enough to automate or manually repeat the command call to force the execution of the vulnerable part.  

The flag was captured after 14 attempts.  

Impact:  

An attacker can extract secrets by simple persistence, without possessing the encryption keys or access normally required.  

Remediation:  

Remove sensitive character strings from production binaries.  

Replace probability logic with strict permission checking. 

 

 

 

 