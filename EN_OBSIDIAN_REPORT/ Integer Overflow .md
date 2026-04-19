Vulnerability #8: Integer Overflow / Underflow 

 

Flag : {ERR0R TURB1NE CAN'T ST0P} 

Severity: Critical  

Type: Integer Overflow / Underflow  

  

Description:  

The run_turbine function suffers from an integer type validation error (signed vs unsigned). The program checks that the user input is less than 15, but it does not exclude negative numbers. This allows the logic to be diverted from the turbine control loop.  

  

Technical Analysis: 

 

Entry and conversion: The user enters a value that is converted to a 32-bit signed integer (int32_t) via atoi.  

Permissive validation: The if condition (!rax_3 || rax_3 <= 0xf) allows any negative number (e.g.: -1), because it is mathematically less than 15.  

The loop is defined by for (; i!= rax_3; i += 1).  

If you enter -1, as the index i starts at 0 and only increases, it will never encounter -1 before reaching its maximum limit (2 147 483 647).  

At this stage, the integer "overflows" and leaves from its lowest value (-2,147,483,648) to finally go back up to -1.  

Flag Trigger: During this "infinite" loop caused by the overflow, the index i quickly exceeds the value 15 (0xf). The if condition (i > 0xf) becomes true, causing the debug secret to appear.  

PoC:  

Execute . /obsidian.  

Launch the run_turbine function.  

At the command prompt, enter: -100, for example.  

Wait a few seconds for the counter to increment; the flag displays as soon as I reach 16.  

Impact:  

This flaw allows an attacker to bypass the reactor’s safety restrictions. By forcing an int overflow, it can trigger debug features or critical error messages that should never be accessible in production, simulating a total loss of control of the turbine.  

Patch:  

Type change: Use an unsigned type (uint32_t) for rotations.  

Secured all possibilities by replacing the condition with if (rax_3 > = 1 && rax_3 <= 15).  

Use i < rax_3 instead of i!= rax_3 to avoid infinite loops in case of unexpected entry. 

