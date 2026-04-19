# Vulnerability #11: Int Overflow 

## Flag {12EXPLOSION34} 

 

The set_reactor_power command is vulnerable to attack by Int Overflow. This flaw occurs when the program transmits a size close to the size of an int  

Suspected vulnerable code: printf(user_input);  

Instead of treating the input as plain text, the program interprets special characters (such as %s, %p, %x) as read or write commands to memory.  

Technical Analysis  

The program accepts an integer value to adjust the reactor power but does not properly check whether this value remains within a safe range before using it. When a value close to the maximum limit of an integer is provided, the system goes into an unstable state: alert messages follow one another, and the reactor simulates a critical temperature rise.  

PoC:  

    Launch the set_reactor_power module.  

    Enter: 2147483646 

In this case, sensitive data associated with the internal operation of the program is exposed because of this mismanagement of integer values. In real-life situations, this type of behavior could lead to the disclosure of critical information related to the internal state of the program, such as controlling data or security information.  

Impact  

The reactor goes into critical condition when a value too high is supplied as input. The program does not properly check the maximum terminals before applying this value to the system.  

Remediation  

Never pass a critical variable without control.  

Systematically use a verification:  

if (user_input > seuil_critical){  

puts("Value too high");  

return 0;  

} 

 

 

 

 