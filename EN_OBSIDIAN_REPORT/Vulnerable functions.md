# Vulnerability functions
## Flag {The stone isn't in the pocket anymore ...} 

The monitor_radiation_levels command is vulnerable to a buffer overflow attack. This vulnerability occurs when the program transmits data without size limit.  

Suspected vulnerable code : gets(buff[10]);  

The fact that we can modify data directly from the buff allows us to modify other variables.  

Technical Analysis  

By injecting the memory address of a function into a function pointer, we replace the initial value of this pointer with another address. So, instead of calling the intended function, the program executes the one whose address has been injected. This allows to divert the flow of program execution.  

PoC:  

    make a C file with the code: 

    compile the code  

    execute the code: a.out | . /obsidian 

The program contains an overflow vulnerability. At runtime, this flaw allows you to write beyond the limits of a buffer and modify data located in adjacent memory, such as a function pointer. This pointer can then be replaced by an address controlled via the data sent to the program.  

The use of an intermediate program makes it possible to generate and send raw bytes, impossible to enter correctly with the keyboard, in particular non-printable values or not representable in standard text.  

The pipe (|) is then used to redirect the output of one program towards the input of another, to directly transmit this raw data to the target program without going through an interactive input from the terminal.  

Impact:  

Execution of functions not provided for by the program  

Execution Flow Misappropriation  

Ability to take control of program behavior  

Remediation  

Never use gets to retrieve and give them because it is controlled by the user. Always use a fget with the max less than the buffer size:  

fget(buff, size_buff, 1); 

 

![alt text](<../Images/The stone isn't in the pocket anymore.png>)