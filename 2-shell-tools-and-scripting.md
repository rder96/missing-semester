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
   - to check not equal: ```if [[ $? -ne 0]]``` or equal (-e)

7. globbing: 
   - wildcards: ? (1 char) or * (any no. of char)
   - {}: whenever have common substring
     - e.g. ```convert image.{pmg,jpg}; touch {foo,bar}/{a..h}}``` -> will get all combis

8. shebang - it is good to write it with the env command that will resolve to wherever the command lives in the system
   - ```#!usr/local/bin/python``` => ```#!/usr/bin/env python``` - will make use of the PATH env variable
  
# Useful Tools
- shellcheck - find errors in sh/bash scripts (brew install shellcheck)
- tldr pages - simplified man pages with practical examples
- find (pre-installed) - recursively research for files matching some criteria
  - ```find . -name src -type d``` - find at currect directory, name = src, type = directory
  - find can also perform actions over files that match query
  - ```find . -name '*.tmp' -exec rm {} \;``` - delete all files with .tmp extension
  - ```find . -name '*.png' -exec convert {} {}.jpeg \;``` 
  > - beware of the syntax
- however, find's syntax is hard to remember -> find -name '*PATTERN*' or (-iname for case insensitive)
  - fd is simple, fast, user-friendly alternative
  - fd PATTERN
- instead of find or fd, can use index/ database for quick searching
  - locate -> uses an index/database for quickly searching 
  - update database using ```updatedb```, in most systems, updatedb is updated daily via cron
  - trade-off = speed vs freshness
  - locate just uses the file name whereas find and fd can also find files using attributes (file size, modification
    time, file permissions)
- grep (-C get Context around the matching line, -v for invert match(all lines that do not match the pattern), -R recursive search)
  - alternatives: ack, ag, rg
  - rg (ripgrep) - fast and intuitive
    - e.g. ```rg -t py 'import requests'```
    - e.g. ```rg -u --files-without-match "#!"``` - -u include hidden files, --files-without-match not match
    - e.g. rg --stats PATTERN
- finding shell commands
  - history
  - Ctrl+R
  - if start a command with a leading space, it won't be added to shell history, handy when typing commands with passwords or orther bits of sensitive information. If want to manually remove the entry, can also edit .bash_history or ,zhistory
- Directory Navigation
  - tree (for Mac, brew install tree)/ broot/ nnn
  - fasd/ autojump 

# Exercises
1. Read man ls and write an ls command that lists files in the following manner
   - Includes all files, including hidden files
   - Sizes are listed in human readable format (e.g. 454M instead of 454279954)
   - Files are ordered by recency
   - Output is colorized
```
ls -lahtG
```
   > -l: List in long format\
   > -a: include all files\
   > -h: uses unit suffixes\
   > -t: sort by time modified\
   > -G: colorized output

2. Write bash functions marco and polo that do the following. Whenever you execute marco the current working directory should be saved in some manner, then when you execute polo, no matter what directory you are in, polo should cd you back to the directory where you executed marco. For ease of debugging you can write the code in a file marco.sh and (re)load the definitions to your shell by executing source marco.sh.
```
#!/bin/bash
marco () {
        echo "current working directory is $(pwd)"
        curr="$(pwd)"
}
polo () {
        cd $curr
}
```
> variables in shell has dynamic scope - need local decl ```local var='inital value'```
> remember to use $curr to get the value of curr

3. Say you have a command that fails rarely. In order to debug it you need to capture its output but it can be time consuming to get a failure run. Write a bash script that runs the following script until it fails and captures its standard output and error streams to files and prints everything at the end. Bonus points if you can also report how many runs it took for the script to fail.
   ```
   #!/usr/bin/env bash
    
   n=$(( RANDOM % 100 ))

   if [[ n -eq 42 ]]; then
    echo "Something went wrong"
    >&2 echo "The error was using magic numbers"
    exit 1
   fi

   echo "Everything went according to plan"
   ```
```
#!/bin/bash
# reference: https://www.codegrepper.com/code-examples/shell/How+to+call+one+shell+script+from+another+shell+script%3F
COUNT=0
> TMPFILE
while :; do
        ./script.sh >> TMPFILE 2>&1
        if [[ "$?" -ne 0 ]]; then
                break
        fi
        COUNT=$((COUNT+1))
done

cat TMPFILE
echo "Total runs needed is $COUNT"
```
> - create a new script named script.sh and copy the above script into it using vim
> - create a temp file in bash using ```TMPFILE=$(mktemp)``` https://www.pixelstech.net/article/1577768087-Create-temp-file-in-Bash-using-mktemp-and-trap
> - : always return true - can be used to do infinite while loop ```while :; do .... done```
> - redirect stdout and stderr to file: https://tldp.org/LDP/abs/html/io-redirection.html 2(stderr) is redirected to where 1 is redirected to.
> - NOTE: Remember by default new script are created with only read and write permission. So, use chmod u+x script.sh
> - used ```> TMPFILE``` to empty the content of TMPFILE if it exists, ```echo -n FILENAME``` doesn't automatically work on zsh https://stackoverflow.com/questions/42344265/echo-n-is-not-working-in-zsh

4. As we covered in the lecture find’s -exec can be very powerful for performing operations over the files we are searching for. However, what if we want to do something with all the files, like creating a zip file? As you have seen so far commands will take input from both arguments and STDIN. When piping commands, we are connecting STDOUT to STDIN, but some commands like tar take inputs from arguments. To bridge this disconnect there’s the xargs command which will execute a command using STDIN as arguments. For example ls | xargs rm will delete the files in the current directory.

Your task is to write a command that recursively finds all HTML files in the folder and makes a zip with them. Note that your command should work even if the files have spaces (hint: check -d flag for xargs).

If you’re on macOS, note that the default BSD find is different from the one included in GNU coreutils. You can use -print0 on find and the -0 flag on xargs. As a macOS user, you should be aware that command-line utilities shipped with macOS may differ from the GNU counterparts; you can install the GNU versions if you like by using brew.
> - xargs -- build and execute commmand lines from stdin
> - downloaded GNU's coreutils
