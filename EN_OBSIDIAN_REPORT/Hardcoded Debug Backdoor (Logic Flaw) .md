# Vulnerability #7 : Hardcoded Debug Backdoor (Logic Flaw) 

## Flag : {ERR0R TURBINE WILL EXPLODE} 

Gravité : Critique 

Type : Logic Flaw 

 

Description:  

The turbine temperature management function (turbine_temperature) contains a code branch that bypasses the normal simulation. It accepts specific numerical values that trigger an immediate and irreversible state of instability in the reactor.  

 

Technical Analysis:  

We used Ghidra and Binary Ninja as a complement for an additional analysis of the two reverse engineering software programs, Pseudo-C / HLIL  

Parsing the obsidian.so library at 004017b5 reveals the following implementation:  

Input retrieval: The program reads user input via fgets and converts it to a 64-bit integer with strtoll.  

The Condition: At address 0x0040185b, the rax_3 register is compared to two boundary constants:  

0x7ffffffe (2147483646): Maximum of a 32-bit integer minus 1.  

0x80000001 (−2147483647): The minimum of a 32-bit integer plus 1.  

Trigger: If one of these values is entered, the program ignores the normal flow, displays an instability message, spits out the security flag and forces the process to stop via exit(1).  

PoC:  

Launch the binary . /obsidian.  

Access the turbine management menu via .  

Select the option to change temperature.  

Enter value: 2147483646.  

Flag display: {ERR0R TURBINE WILL EXPLODE}.  

Impact:  

This vulnerability allows an informed user to cause a system emergency shutdown or simulate a catastrophic explosion.  

Removing dead code: Remove comparisons with arbitrary constants used for development.  

Input validation: Implement realistic temperature limits, e.g. -50 to +150 degrees, and reject any value outside these limits before proceeding further. 

 ![alt text](<../Images/TURBINE WILL EXPLODE.png>)