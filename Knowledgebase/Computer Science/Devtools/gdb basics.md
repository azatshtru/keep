The GNU Debugger used to debug your C code.
### Stepping over vs stepping into   
Stepping over refers to going over the lines of code. If the debugger is currently on top of a function call, stepping over at that moment will run the function and the debugger will move on to analyze the next line.   
Stepping into refers to looking closely at a function call and looking at the instructions inside that function. If the debugger is currently on top of a function call, stepping into will make the code jump to the function and debugger will start analyzing the lines and instructions inside that function.   
Stepping out refers to going back to where the function was initially called.   
### Some common gdb commands   
To compile code with debug information that helps gdb in debugging your code, use the `-g` flag during gcc compilation.   
Running `gdb executable_name` will start gdb   
Some gdb commands:   
1. `break function_name` will put a breakpoint at the start of the function.   
2. `run` will start execution of program in debug mode.   
3. `next` : step over c code   
4. `nexti` : step over assembly instructions generated from c code.   
5. `step` : step into the function that is being called.   
6. `ref` refresh gdb UI   
7. `x/i $pc` will lead to the exact assembly instruction that is running right now. If this is called when the code inevitably crashes, then this will show the exact assembly instruction that caused the program to crash. x/i stands for examine instruction, and `pc`  is the program counter register's value.   
8. `info registers` will give the current states of registers and the values inside registers.   
   
   
### Additional debugging with core dumps   
Core dumps are the copies of the exact state of the memory when the program crashed. The usual limit for core dumps in computers is zero, so that every crash doesn't log itself. The size of core dump can be checked by `ulimit -c` and can be set to infinity by `ulimit -c unlimited`.   
Core dumps can be used with gdb by running a faulty program in debug mode (\`-g\`), and then running `gdb ./program_executabe ./core`    

> On MacOS X, one can use lldb, which used roughly the same command structure as gdb,
> - `lldb executable_name` for loading the executable into the debugger
> - `r` to run executable inside the debugger
> - `br se -f file_name -l line_number --name function_name` to set breakpoints
> - `s` to step into
> - `n` to step over
> - `f` to step out
> - `frame variable` to look at the state of the stack during execution