#### Basics for Running Bash Scripts ####
1. Scripts start with #!/bin/bash. The "#!" tells bash it's a script. "#!" is called a shebang, short for hash bang. The "/bin/bash" tells bash which interpreter to run it in.
2. After saving your script make it executable with chmod. (`chmod 777 filename` makes it executable by *everyone*).
3. Execute the script from the current directory with `./script_name.sh`. You can only type script_name and get it to run if you current directory is in the $PATH, by typing `./script_name.sh` you're telling it specifically where it is so it doesn't need to be in the path.

#### Actual Programming Syntax ####
Notes From http://linuxsig.org/files/bash_scripting.html

##### Assigning Variables #####
`variableName=1` or `variableName=String` <= Note the deliberate lack of space before and after the =. Also, not how the string didn't require quotes. 

##### Accessing Variables #####
Retrieving variables requires a $ before the name, so $variableName.

##### Built-in Shell Variables #####
- $# Stores the number of command-line arguments that were passed to the shell program. Example if you ran `test.sh wordywords morewords`, within test.sh `$1` would return "wordyworks" and `$2` would return "morewords".
- $? Stores the exit value of the last command that was executed.
- $0 Stores the first word of the entered command (the name of the shell program).
- $* Stores all the arguments that were entered as a single word ex. "$1 $2 ..."
- $@ Stores all the arguments that were entered on the command line, individually quoted ("$1" "$2" ...) as seperate strings. Slightly different than previous, apparently has different effects when passing it to other programs.

##### Quotes #####
- Double Quotes: `longStringVariable="string with spaces"` works , but bash with evalaute anything it recognizes within the double quotes (for example a variable name would be evaluated).
- Single quotes: (') are the most literal quoting and won't evaluate anything inside the quotes.
- Escape Character: (\) escape a single character to prevent bash from evaluating it. Useful for things like `price=\$5.00`.

```bash
# Example of Different Bash Quotings
MYVAR = 1
evaluatingQuotes = "$MYVAR"   ----> Returns 1
literalQuotes = '$MYVAR' ---------> Returns $MYVAR
escapeCharacter = "/$MYVAR" ------> Retursn $MYVAR
```

Escape Character (\) will escape a single character, like if you wanted to use price=\$5.00

###### Back-ticks (\`) ######
You can use back-ticks (\`) in scripts to run commands and store the results for later use, example: "var=\`ls\`" or "data=\`date\`".

#### test command ####
A command called test is used to evaluate conditional expressions. You would typically use the test command to evaluate a condition that is used in a conditional statement or to evaluate the entrance or exit criteria for an iteration statement. The test command has the following syntax:

```bash
test expressions
or
[ expressions ]
```

##### Comparison Expressions #####
- Equal To:                `int1 -eq int2` Returns True if int1 is equal to int2.
- Greater Than or Equal To `int1 -ge int2` Returns True if int1 is greater than or equal to int2.
- Greater Than:            `int1 -gt int2` Returns True if int1 is greater than int2.
- Less Than or Equal To:   `int1 -le int2` Returns True if int1 is less than or equal to int2.
- Less Than:               `int1 -lt int2` Returns True if int1 is less than int2.
- Not Equal To:            `int1 -ne int2` Returns True if int1 is not equal to int2.

##### Logical Expressions #####
- `str1 = str2` Returns True if str1 is identical to str2.
- `str1 != str2` Returns True if str1 is not identical to str2.
- `str` Returns True if str is not null.
- `-n str` Returns True if the length of str is greater than zero.
- `-z str` Returns True if the length of str is equal to zero.

##### File Tests #####
- `-d filename` Returns True if file, filename is a directory.
- `-f filename` Returns True if file, filename is an ordinary file.
- `-r filename` Returns True if file, filename can be read by the process.
- `-s filename` Returns True if file, filename has a nonzero length.
- `-w filename` Returns True if file, filename can be written by the process.
- `-x filename` Returns True if file, filename is executable.

##### Extra Logical Expressions #####
- `! expr` Returns True if expr is not true.
- `expr1 -a expr2` Returns True if expr1 and expr2 are true.
- `expr1 -o expr2` Returns True if expr1 or expr2 is true.

#### Conditional Statements ####
##### If Statements #####

```bash
if [ expression ]
then
      commands
elif [ expression2 ]
then         #OPTIONAL
      commands
else          #OPTIONAL
      commands
fi
```

##### Case Statements #####
```
case string1 in
str1)
    commands;;
str2)
    commands;;
*)
    commands;;
esac
```

Example (remember the $1 gets quoted to turn it into a string, so the option is a string:
case $1 in-i)   count='grep ^i $2 | wc -l'   echo "The number of lines in $2 that start with an i is $count"   ;;-e)   count='grep ^e $2 | wc -l'   echo "The number of lines in $2 that start with an e is $count"   ;;*)   echo "That option is not recognized"   ;;esac
Loopsfor loopThe first form of the for statement. In this form, the for statement executes once for each item in the list. This list can be a variable that contains several words separated by spaces, or it can be a list of values that is typed directly into the statement. Each time through the loop, the variable var1 is assigned the current item in the list, until the last one is reached:
for var1 in list
do
   commands
done

Below is the second form. For this form, the for statement executes once for each item in the variable var1. When this syntax of the for statement is used, the shell program assumes that the var1 variable contains all the positional parameters that were passed in to the shell program on the command line. 
for var1
do
   statements
done

The above is equivalent of the below:
for var1 in "$@"
do
   statements
done

The following is an example of the for statement. This example takes as command-line options any number of text files. The program reads in each of these files, converts all the letters to uppercase, and then stores the results in a file of the same name but with a .caps extension.
for file
do
   tr a-z A-Z < $file >$file.caps
done

while loopThis statement causes a block of code to be executed while a provided conditional expression is true. The syntax for the while statement is the following:
while expression
do
   statements
done

Example program that lists the parameters that were passed to the program, along with the parameter number.
count=1
while [ -n "$*" ]                                #-n tests whether non-zero in length
do
   echo "This is parameter number $count $1"
   shift                                         #Shift command moves the command-line parameters over one to the left.
   count='expr $count + 1'
done

until loop
Opposite of while loop. The until statement executes its code block while its conditional expression is false, and the while statement executes its code block while its conditional expression is true. Syntax:
until expression
do
   commands
done

The same example that was used for the while statement can be used for the until statement. All you have to do to make it work is negate the condition. This is shown in the following code:
count=1
until [ -z "$*" ]                                #-z tests whether zero in length
do
   echo "This is parameter number $count $1"
   shift
   count='expr $count + 1'
done

The only difference between this example and the while statement example is that the -n test was changed to a  -z test option (test whether non-zero changed to test whether zero). In practice the until statement is not very useful, because it's likely you'll just use the more common while loop.
The shift CommandThe shift command moves the current values stored in the positional parameters to the left one position. For example, if the values of the current positional parameters are:
$1 = -r 
$2 = file1 
$3 = file2

and you executed the shift command the resulting positional parameters would be as follows:
$1 = file1 
$2 = file2

You can also move the positional parameters over more than one place by specifying a number with the shift command. This is a very useful command when you have a shell program that needs to parse command-line options. This is true because options are typically preceded by a hyphen and a letter that indicates what the option is to be used for. Because options are usually processed in a loop of some kind, you often want to skip to the next positional parameter once you have identified which option should be coming next. For example, the following shell program expects two command-line options, one that specifies an input file and one that specifies an output file. The program reads the input file, translates all the characters in the input file into uppercase, then stores the results in the specified output file.
while [ "$1" ]
do
   if [ "$1" = "-i" ]; then
      infile="$2"
      shift 2
   elif [ "$1" = "-o" ] 
   then
      outfile="$2"
      shift 2
   else
      echo "Program $0 does not recognize option $1"
   fi
done

tr a-z A-Z < $infile > $outfile

FunctionsSyntax for function creation:
fname () {
   shell commands
}

Once you have defined your function using one of these forms, you can invoke it by entering the following command:
fname [parm1 parm2 parm3 ...]

Notice that you can pass any number of parameters to your function. When you do pass parameters to a function, it sees those parameters as positional parameters, just as a shell program does when you pass it parameters on the command line.
The following shell program contains several functions, each of which is performing a task associated with one of the command-line options. This example illustrates many of the topics covered in this section. It reads all the files that are passed on the command line and—depending on the option that was used—writes the files out in all uppercase letters, writes the files out in all lowercase letters, or prints the files.
upper () {
   shift

   for i
   do
      tr a-z A-Z < $1 > $1.out
      rm $1
      mv $1.out $1
      shift
   done;
}

lower () {
   shift

   for i
   do
      tr A-Z a-z < $1 > $1.out
      rm $1
      mv $1.out $1
      shift
   done;
}

print () {
   shift

   for i
   do
      lpr $1
      shift
   done;
}

usage_error () {
   echo "$1 syntax is $1 <option> <input files>"
   echo ""
   echo "where option is one of the following"
   echo "p to print frame files"
   echo "u to save as uppercase"
   echo "l to save as lowercase"
; }

case $1
in
   p | -p) print $@;;
   u | -u) upper $@;;
   l | -l) lower $@;;
   *) usage_error $0;;
esac
;}

