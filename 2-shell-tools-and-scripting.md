# Shell Tools and Scripting
- Shell Scripting (bash scripting for this)
  - shell scripting different from other scripting programming language - it is optimized for performing shell-related tasks
  
1. - To assign variables in bash, use ```foo=bar```\
   - To accesss the value, use ```$foo```
   > NOTE: foo - bar not work, interpreted as calling foo program, arguments = and bar\
   > In general, space character perform argument splitting

2. ```'``` delimited strings are literal strings, will not substitute variable values vs ```"``` delimited strings will.

3. Example of function: 
```
mcd () { 
    mkdir -p "$1"
    cd "$1" 
} 
```
> - $0 - Name of the script 
> - $1 to $9 - Arguments to the script.
> - $@ - All the arguments
> - $# - Number of arguments
> - $? - Return code of the previous command(0 = run successfully, 1 = error)
> - $$ - Process identification number (PID) for the current script
> - !! - Entire last command, including arguments, e.g. to quickly re-execute the command with failed permission with sudo by doing sudo !!
> - $_ - Last argument from the last command. If you are in an interactive shell, Esc followed by . works too

4. - short-circuiting operators (&& and ||), separate commands with semicolon```;``` (all commands will run)
   > - false || echo "Oops, fail"
   >   - will print since false is 0, will execute second command
   > - true || echo "Will not be printed"
   >   - will not print since true is 1
   > - true ; echo "This will always run" 
   >   - as mentioned

5. - Command substitution: 
     - whenever you place ```$( CMD )```, it will execute the output of the command and substitute it in place
     - e.g. ```for file in $(ls)``` will iterate over all values of output of ls
   - Process substitution
     - ```<( CMD )``` will execute CMD and place the output in a tmp, sub ```<()``` with the file's name
     - useful when commands expect values to be **passed by file** instead of by STDIN
     - ```diff <(ls foo) <(ls bar)```
     
6. When doing comparisons in bash, try use ```[[]]``` instead of ```[]```.
   - to check not equal: ```if [[ $? -ne 0]]```

7. globbing: 
   - wildcards: ? (1 char) or * (any no. of char)
   - {}: whenever have common substring
     - e.g. ```convert image.{pmg,jpg}; touch {foo,bar}/{a..h}}``` -> will get all combis

8. shebang - it is good to write it with the env command that will resolve to wherever the command lives in the system
   - ```#!usr/local/bin/python``` => ```#!/usr/bin/env python``` - will make use of the PATH env variable
  
# Useful Tools
- shellcheck - find errors in sh/bash scripts (brew install shellcheck)
- 
