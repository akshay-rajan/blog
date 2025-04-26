---
title: "Shell Scripting"
permalink: /shellscripting
---


# Bash Scripting

### 0. Execution

    ./filename.sh
    sh filename.sh
    source filename.sh

### 1. Shebang

    #!/bin/bash

### 2. Display

    echo "Hello, World"

### 3. Comments

    # This is a comment

### 4. Here Documnet (here-doc)

To input multiple lines of text to a command

    command << delimeter
        text...
        statements...
    delimeter

### 5. `if` Statement

    if [ condition ]
    then
        statements
    elif [ condition ]
    then
        statements
    else
        statements    
    fi

If we are using signs (`>`, `<`, `=`, ...) instead of letters (`-gt`, `-lt`, `-eq`, ...) in the condition, use (()) instead.

    if (( condition ))

We can use logical operators like

    if [ condition1 ] && [ condition2 ]
                    or
    if [[ condition1 && condition2 ]]


These can also be implemented using the corresponding letters for the logical operators:

    if [ condition1 -a condition2 ]

### 6. `case` Statement

    case expression in
        pattern1)
            commands
            ;;
        pattern2)
            commands
            ;;
        *)
            default commands
            ;;
    esac

### 7. `while` loop

The while loop is executed  as long as the condition evaluates to true.

    while [ condition_is_true ]
    do 
        statements
    done


### 8. `until` loop

The until loop is executed as many times the condition or command evaluates to false.
The loop terminates when the condition or command becomes true.

    until [ condition_becomes_true ]
    do
        statements
    done

### 9. `for` loop

    for variable in {start...stop...step}
    do 
        statements
    done

Or we can do

    for (( i=0; i<5; i++ ))

### 10. `break` and `continue`

    for (( i=0; i<=10; i++ ))
    do
        if [ $i -eq 5 ]
        then 
            continue
        fi
        echo $i
    done

### 11. Script Input: STDIN

**Command-line Arguments**

    ./filename.sh input1 input2 input3
        ↑           ↑       ↑     ↑
        $0          $1      $2    $3

We can use any number of command-line arguments given, by using an array

    args=("$@")
    echo [args[0]]

**Reading a file**

Reading a file using the filename

    while read line
    do 
        echo $line
    done < "{$1:-/dev/stdin}"

To run

    ./filename.sh   file_to_read

If we give no filename as the argument, the program will consider the terminal as the file, and the input given via the command line will be taken as *$line*.

### 12. Script Output: STDOUT, STDERR

We can redirect the Standard Output and the Standard Error after running a command to two different files.

    command 1>file1.txt 2>file2.txt
            ↑           ↑
          STDOUT      STDERR
            ↓
    command >file.txt

To redirect the output and error to the same file, do

    command 1>file1.txt 2>$1
            or
    command >& file1.txt

### 13. Send the output of one script to another 

In `script1`,
    
    x = 1234
    export x
    ./script2.sh

In `script2`,

    echo $x

### 14. String Processing

* We can compare 2 strings using `==`
* We can compare their lengths using `\>` and `/>`
* Use `^` for lowercase and `^^` for uppercase

### 15. Numbers and Arithmetic

    n1 = 4
    n2 = 5
    echo $(( n1 + n2 ))
    echo $(( n1 - n2 ))
    echo $(( n1 * n2 ))
    echo $(( n1 / n2 ))
    echo $(( n1 % n2 ))

        or

    echo $(expr $n1 + $n2 )
    echo $(expr $n1 \* $n2 ) 
    ...

To convert a hexadecimal number to decimal,

    hexa=f23a2b
    echo "obase=10; ibase=16; $hexa" | bc  

### 16. Declare command

`declare` command is used to update variables within the shell.

We can see all variables in the system by running `declare -p`
To create a variable, 

    declare myvariable

To give a value to it,

    declare myvariable=12

To create a read only variable, do

    declare -r myvariable="value"

### 17. Arrays

To create an array and print out the entire array,

    name = ('element1' 'element2' 'element3')
    echo "${name[@]}"

To print the element at a particular index, do

    echo "${name[1]}"

To print the indexes in the array, do

    echo "${!name[@]}"

To get the length of an array, 

    echo "${#name[@]}"

To remove the value at an index, run

    unset name[2]

To insert a value at a particular index, do

    name[index]=value

We can use `seq` to get a sequence of values

    seq [last]
    seq [first] [last]
    seq [first] [step] [last]

For example, do

    for i in $(seq 5) 
    do 
        echo $i
    done

### 18. Functions

To declare a function,

    function funcName()
    {
        statements...
    }

Or

    funcName() {
        statements...
    }

To call a function, run

    funcName

We can use positional arguments in a function

    funcName() {
        echo $1 $2
    }

    funcName "Hello" "World"

### 19. Files and Directories

To create a directory, run

    mkdir -p dir_name

To check if a directory exists, do

    if [ -d $dir_name ]
    ...

To create an empty file, run `touch filename`.

To chek if a file exists, do

    if [ -f $file_name ]
    ...

To overwrite a file or append to an existing file, respectively do

    echo "$filename" > "Something to overwrite"
    echo "$filename" >> "Something to append"

To read line by line from a file,

    while IFS= read -r line
    do
        echo "$line"
    done < $filename

To delete a file, do

    rm filename

### 20. Sending Email

1. Install ssmtp

        sudo apt install ssmtp

2. Edit the file `/etc/ssmtp/ssmtp.conf`

        root=email@123.com
        mailhub=ssmtp.gmail.com:587
        AuthUser=email@123.com
        AuthPass=
        UseSTARTTLS=yes

3. Run 

        ssmtp <email>
    
4. Write the mail.

        To:
        From:
        Cc:
        Subject:
        this is the body of the mail.


### 21. `curl`

To download a file, run

    curl <url> -O 

To rename the file

    curl <url> -o new_name
            or
    curl <url> > new_name

To download the header, run

    curl -I <url>

### 22. `select` loop

    select variable_name in option1 option2 option3 option4
    do 
        echo $variable_name
    done

To avoid invalid inputs, we can use `switch` to do

    select ...
    do 
        case $name in 
        option1)
            echo option1
        ...
    done

To wait for a keypress,

    while [ true ]
    do 
        read -t 3 -n 1
    if [ $? = 0 ]
    then 
        exit;
    else
        echo "Waiting for keypress"
    fi

### 23. inotify

Inode Notify is used to monitor the files and directories. 

    inotifywait -m /path/to/directory

This constantly notifies us of any operation inside our directory being `watched`.

### 24. grep

**Global Regular ExPression** is used for searching text for lines that match a regular expression.

    grep <regex> <file>

Searching case insensitively,

    grep -i $grepVar $filename

For the count or line number, do respectively

    grep -c ...
    grep -n ...

### 25. awk

`awk` is a programming language used to manipulate data. 
awk allows us to search for a particular pattern, and also to specify the action to be taken when the pattern is found.
We can run `awk` inside bash.
To print using awk, do

    awk '{print}' /path/to/filename

To search for a pattern,

    awk '/pattern/ {print}' /path/to/filename

To print out a specific field

    # Prints out the second field: "is" from "This is linux"
    awk '/linux/ {print $2}' $filename
    
    # Fields 3 and 4
    awk '/linux/ {print $3, $4}' $filename

### 26. sed

Stream EDitor is used to manipulate text files.
To substitute all occurences of a 'i' with 'I' and display it, do

    cat filename.txt | sed 's/i/I/'
                or
    sed 's/i/I/g' <filename>

To modify the file, we need the `i` switch

    sed -i 's/i/I/g' <filename>

### 27. comm

The `comm` command is used to compare two files line by line. 

    comm [OPTION]... FILE1 FILE2

For example, to compare two sorted files named file1.txt and file2.txt and display only the lines unique to file1.txt, you can use:

    comm -23 file1.txt file2.txt

Similarly, to display only the lines common to both files, you can use:

    comm -12 file1.txt file2.txt

### 28. Debugging

1. Run 

        bash -x ./filename.sh

    Which enables us to see step-by-step which lines are being executed.

2.  We can also do this by adding `-x` to the shebang

        #!/bin/bash -x

3. We can select the lines to be executed by

        set -x
            ...
            statements to be executed
            ...
        set +x



