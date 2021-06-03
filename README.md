# Shell Scripting

## Introduction
it is interpreted and not compiled
supported shells in your computer:
cat /etc/shells
sh shell is the original shell and bash is born again shell which is more intuitive and defult in gnu which is instructed here.
to see where bash is located: ```which bash```

to write bash script start with ```#! /bin/bash (location of you bash)```
this is to tell the script where to find bash.
The original file created by touch is not executable --> ```chmod +x hello.sh```

\# is used for comment which would not be executed.

## variables
to store data like string, integer.
1. system variables: defined by Linux in capital letter -> $BASH, $BASH_VERSION, $HOME, $PWD
2. user defined variables
```
name=fateme
echo $name
```

## Read User Input
```
echo "enter your name: "
read name1 name2 name3
echo $name1 $name3
```
keeps the cursor at the line
```
read -p "username: " username
```
not showing what user entered(password):  `read -s`
read into array `read -a names`
if we only have ```read``` then the entered value is saved into `$REPLY`

## Pass argument
1.
`echo $1 $2 $3`
`$0` --> name of shell script
2. pass argument through array
```
arg=("$@")
echo {$arg[0]} --> first element not filename
echo $@ --> all elements
echo $# --> number of elements
```


## if statement
**Example:**
```
if [condition]
then
    echo "condition a is true"
elif [condition]
then
    echo "condition b is true"
else
    echo "condition is false"
fi
```

#### integer comparison:
1. -eq in []
2. -ne
3. -gt
4. -ge
5. -lt
6. -le
7. < =< >= > in (())

#### string comparison:
1. = (equality) in []
2. ==
3. !=
4. <> in [[]]    
5. -z string in null


## File test operator
ceck if file exists, has a certain format...
```
echo -e "enter the name of file: \c"
read filename
```
-e \c keep cursor at the line

1. check if file exists: -f
 ```
    if [ -e $filename ]
    then
        echo "$filename exists"
    else
        echo "$filename not found"
    fi
```
2. check directory: -d
3. check binary file: -b
4. check charcter based file: -c
5. check not empty file: -s
6. has read/write/execute permission: -r/-w/-x


## Append or write text to a file
```
if [ -f $filename ]
then
    if [ -w $filename ]
    then
        echo "write something. to exist press ctrl+d"
        cat >> $filename  --> to append
        cat > $filename  --> to overwrite the file
    else
        echo "$filename doesn't have write permission"
    fi
else
    echo "$filename doesn't exist"
fi
```

## logical AND
1. && with two brakets
2. && with double braket
3. -a
```
age=25
if [ $age -gt 18 ] && [ $age -lt 30 ]
if [ &age -gt 18 -a $age -lt 30 ]
if [[ &age -gt 18 && $age -lt 30 ]]
then 
    echo "age is valid"
fi
```
## logical OR
1. || with two brakets
2. || with double brackets
3. -o

## arithmetic operation
```echo 1+1```
just print 1+1 which takes as string and doesn't do arithmetic operations.
1. $, double paranthesis, space:
```
num1=5
num=20
echo $(( num1 + num2 ))
```
2. $, single paranthesis, expr, $
```
echo $(expr $num1 + $num2)
**for multiplication:**
echo $(expr $num1 \* $num2)
```

## floating point operation
doing the similar operation as above raises syntax error for non integer values.
bc: a language for precision calculation
``` echo "20.5+1.5" | bc```
**| is called pipeline**
Division problem --> gives integer result. To overcome this problem:
```echo "scale=2;20.5/1.5" | bc```

bc can do more than arithmetic operations like power, square root & etc. check for man bc for more info.
we need to use math library for these operations.
example: 
``` echo "scale=2;sqrt(25)" | bc --mathlib (or -l)```

## case statement
```
vehicle=$1
case $vehicle in
    "car" )
        echo "Rent of $vehicle is 100 dollar" ;;
    "bus" )
        echo "Rent of $vehicle is 200 dollar" ;;
    "bike" )
        echo "Rent of $vehicle is 5 dollar" ;;
    * )
        echo "Unknown vehicle" ;;    
esac
```
regular expressions 
1. [a-z] small leters
2. [A-Z] capital letters
3. [0-9] digits
4. ? special characters

LANG is language/local which should be set to C if we want to use these regex.

## Array 
create array with paranthesis and space
```os=('linux' 'windows' 'osx')
echo "${os[@]}"
echo "${os[1]}" --> windows
echo "${!os[@]}" --> gives indices
echo "${#os[@]}" --> gives length
```
to append an element:
```
os[3]="ubuntu"
```
to update a value:
```os[0]="kali"```
to remove an element:
```unset os[2]```
gap and uninitialized elements in bash array is alright. we can even write: ```os[10]="mint"``` and it leaves the indices from 3 to 10 unset.


## While loop
```
while [ condition ]
do
    command1
    command2
done
```
**example**
```
n = 0
while [ n -lt 10 ]
do 
    echo $n
    (( n++ ))
done
```
#### using sleep to give delay
```
n = 0
while [ n -lt 10 ]
do 
    echo $n
    (( n++ ))
    sleep 1
done
```
it gives 1 second of sleep to print the next command.

## open new terminal
```
gnome-terminal &
xterm &
```

## Read file content
1. first method
```
while read p
do
    echo $p
done < filename
```
2. second method using pipe
```
cat filename | while read p
do
    echo $p
done
```
3. third method specially used for special characters with IFS
IFS: Internal Field Seperator and in ' ' we say recognize boundaries based on white space
```
while IFS=' ' read -r line
do
    echo $line
done < filename
```


## UNTIL loop
like while loop with small difference: in while loop until the expression is true the commands are executed but in until loop until the expression is false the commands are executed.
```
until [ condition ]
do 
    command1
    command2
done
```

## FOR loop
1. used for variable in an array
2. used to loop inside files
3. used for linux commandline
4. used like a c for loop
```
for VARIABLE in 1 2 3 .. N
for VARIABLE in file1 file2 file3
for VARIABLE in $(Linux command here)
for (( EXP1; EXP2; EXP3))
do
    command1
    command2
done
```
example:
1.
```
for i in 1 2 3
do
    echo $i
done
```
2.
only works for bash version greater than 4
```
echo ${BASH_VERSION}
for i in {1..10} --> {START..END..INCREMENT}
do
    echo $i
done
```
3.
```
for (( i=0; i<5; i++ ))
do
    echo $i
done
```
4.
```
for command in ls pwd date
do
    echo "-------$command-------"
    $command
done
```
5. show all files in a directory
```
for item in *
do
    if [ -f $item ]
    then
        echo $item
    fi
done
```
## select loop
select loop is the same as for loop but provides a menu structure to select.
```
select name in john tom ben
do
    echo "$name selected"
done
```
press ctrl+c to exit the menue.

## BREAK and CONTINUE
break is used to jump out of for loop
continue skips normal execution for that condition


## Function
```
function name(){
    command
}
OR
name(){
    command
}
```
To call a function, call it in the script after function definition.
Sequence is important. and is read line by line.
To pass an argument to a function:
```
function print(){
    echo $1 $2
}
print Hello World
```
by default variables in a function are global. If we want it to be local to the function we should define as: ```local name```

## ternary if
```
[[ -f $filename ]] && return 0 || return 1

```
## readonly command
```
var=15
readonly var
var=50
```

this raises an error that var is readonly

to make a function readonly we need flag -f

readonly --> shows all readonly variables
readonly -f --> shows all readonly functions

## Signals and traps
show PID:
``` echo "$$"```

exit 0 to exit bash script successfully
Interrupt signal --> ctrl+C
ctrl+Z --> suspand signal
kill -9 --> SIGKILL

you can see different signals by man 7 signal

trap is used to catch the signal when a signal is recieved. However it doesn't catch SIGKILL and SIGSTOP.
```
trap "echo exit signal" SIGINTERUPT

exit 0
``` 
example:
```
filename=/tmp/test
trap "rm -f $filename; exit" 0 2 15
```
this removes the filename incase corresponding signals are executed.


## bash debugging
1. when running the script: bash -x ./hello.sh (name of script)
2. set option: #!bin/bash -x at the top of script
3. set -x after !#bin/bash and set +x deactivate debuggin

