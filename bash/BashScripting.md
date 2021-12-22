# Bash Scripting

## The Very Basics
* Comments: `#`
* Basic Output: 
	- `echo "..."`
	- `print "..."`
## Variables:
Hold everything as text
```
	x=13
	x="blah"
```

* Assumes command ends at the end of the line
* Can put multiple commands on one line separated by semicolons
* Can make a command multiple lines by using `\` at the end of line
* Variables are global by default
* They do not need to be declared before being used
* Can make local by using keyword `local`
* If you want to use a variable value later in the script you access id through `${x}`

## Whitespace:
Bash can be finicky.
```
username="Critch646" # fine
directory = "csci265" # bad
```

* Bash can transform, back and fourth, between text and integers in math contexts
* Single quotes in Bash:
```
y=200
x="${y}"  # Looks at value of y and assigns to x
x='${y}'  # is a literal string
```
* Bash has no floating point support.

## Input
```
read x  # read next input value into x
read a b c  # read three values in

read -p "Please enter something" a b c
```

## Integer Math
We have to let Bash know we want things evaluated in integer context/environment (not text)

```
result=$(( (x + y * 3 % 2)**7 ))
let "a=b+c"
```

We can call programs like `bc` to do floating point math and return the result.
```
y=$(( bc <<< "37.5*12/4.7+f" ))
```
## If Statements
```
if [   ] ; then


fi
```

For string comparisons:

```
${a} < ${b}
     >
     <=
     >=
```

For integer comparisons:
```
	-le
	-lt
	-ge
	-gt
	-eq
	-ne
```

### Compound Statements
```
	-a  # AND
	-o  # OR

[ $a -le $b -a $b -le $c ]
	(a < b)&&(b<c)

```

### Alternative Syntax
``` 
if [[ ... && ... ]]  # Allows for traditional compound operators

if [   ] ; then

elif [   ] ; then

else [   ] ; then

fi
```

To negate the condition:

```
if ![   ] ; then

fi
```

## Unary Expressions

```
[ (property to test) {file, dir, string} ]

[ -r "foo" ]  # is foo readable?
[ -w "foo" ]  # is foo writeable?
[ -x "foo" ]  # is foo executable?
[ -n "foo" ]  # is foo NOT NULL?
[ -f "foo" ]  # is foo a file?
[ -d "foo" ]  # is foo a directory?
```

## While Loop

```
while [ ] ; do

done
```

## For Loop
Recommend a C/Java style
```
for (( x=1; x<10; x++ )) ; do

done
```

For loops iterating across a set of values:
```
for x in a b c ; do

done
```

```
for x in ${a} ${b} ${c} ; do

done
```

```
x="foo blah ick"
for i in ${x} ; do

done
```

```
for i in foo blah ick ; do

done
```

```
for i in someArray ; do  # for each element in the array

done
```

### Iterating Across Line of Text
There is a separation variable, IFS.
```
IFS = "\n"
for line in ${text} ; do

done
```

### Run Command, Capture Output Directly, Process One Line at a Time

```
while IFS=read line ; do
	# calling read, putting input value into line and process line. IFS is NULL when read fails.
	
done
```

We want to run some command that reads stuff from standard input usually. Instead we have a string we want to use for its input:

```
mycommand  <<< "... .. .. ... .. ."
```

Suppose the text we want to read is really long:
```
mycommand <<< KEYWORD # Keyword can be anything you like as long as it does not appear inside the text block to read

	"big block of text to
	read instead of stdin"
KEYWORD
```

## Arrays
```
myarray=("foo" ${x})
```
### Assigning to Variables
```
var=${varname} # similar idea with arrays, "normal" indexing syntax on LHS dereference syntax on RHS

# array examples
arr[0]=${arr[1]}
arr[${i}]=${arr[${i}]}
```
Content of whole array, whitespace delimited:
```
${#arr[@]}
```
	
You can increase size of array by assigning to larger positions in the array:

```
arr1=(10 20 30)  # 3 elements
arr2[17]=200  # 18 elements. Uninitialized in most of new chunk.
```

You can unset entire arrays or elements within:
```
unset arr[17]  # unsets element at index 17
unset arr  # unsets all elements of array arr
```

## Command Line Arguments
Provided in an array and we want the count of the number of arguments entered and get each one. `$@` is a special array for command line arguments.

```
for arg in $@ ; do

done
```
To access specific arguments:
```
$1  # first arg
$2  # second arg
$3  # third arg
# etc
```
It is recommended to copy the arguments into named variables and use the variables instead.

### Shift
Used to discard the front element for parameters/list/command line arguments list/array
* Look at front element
* Shift to discard it
* Repeat until out of arguments

```
while [ $# -gt 0 ] ; do
	someArray+=$1
	shift
done
```

# Some Scripts
## Find Files Modified Within a Number of Hours
Find all the files in a directory and its subdirectories that have changed in the last N hours.

```
#! /bin/bash
# Take directory and number of hours as cmd line args
# example: ./findRecent.sh ~/csci265 24

if [ $# -new 2 ] ; then
	echo "correct usage is:"
	echo "$0 [directory] [number of hours]"
else
	directory=$1
	shift
	hours=$1
	shift
	
	((minutes=hours * 60))

	if [ -d ${directory} ] ; then
		echo "files change in that time:"
		find ${directory} -type f -mmin -${minutes}
	else
		echo "invalid directory ${directory}"
	fi
fi
```
## Summarize Git Repo Commits
```
#! /bin/bash
# When in a git repo, get summary of all the commits, 1 line per commit.

if [ -d ".git" ] ; then
	git log --pretty=format:"%h -%an,%ar:%s"
else
	echo "You are not in the root of a git repository"
fi
```

## Display Middle of Text File
```
#! bin/bash
# Display chunk from the middle of a text file given a filename and two line numbers as command line arguments.
# Display the chunk of text file between these lines.

if [ $# -new 3 ] ; then
	echo "Correct usage is:"
	echo "$0 [filename] [startline] [endline]"
else
	# Capture the args into variables
	filename=$1
	shift
	startline=$1
	shift
	endline=$1
	shift

	# Check if filename is a valid file
	if [ -f ${filename} ] ; then
		if [ ${startline} -lt ${endline} ] ; then
			N=0
			# Calculate the difference
			(( N=endline + 1 - startline ))
			head -n ${endline} ${filename} | tail -n ${N}
		else
			echo "${startline} is not less than ${endline}"
		fi
	else
		echo "${filename} not found"
fi
```

## Discard Server-Side Git Repo
It is not unusual to get rid of your server-side repository when using `git submit` (usually due to an error in the `ssh fork` command entered).

You can discard your server stuff with:
```
ssh csci D trash ${course}/$USER/${repo}
```
We want a script to use like `./discard.sh csci265 project` and it should trash if possible.

Plus, if I cloned and messed up repo, then discard repo as well.

```
#! /bin/bash

if [ $# -ne 2 ] ; then
	echo "Correct usage is:"
	echo "$0 [course number] [repo name]"
else
	course=$1
	shift
	repo=$1
	shift
	
	# Create expected path
	myhome=~
	expPath=${myhome}/csci${course}/${repo}
	
	# Check to see if expected path exists
	if [ -d "${expPath}" ] ; then
		rm -rf ${expPath}
	fi
	ssh csci D trash csci${course}/$USER/${repo}
fi
```

# Functions in Bash

```
function funcName( )
{


}
```
* Parameters are in array `$@`
* Number of parameters are in `$#`
* Parameters can be accessed using `$1, $2, $3` etc.
* If not that many parameters passed, then the parameter call will return a NULL string
* To create local variables:
```
# create local named variables to increase readability.
local x=$1
local y=$2
local z=$3
```

* When call the function there are no parenthesis when passing parameters.
* `return` is able to return an int in range 0..255. This is meant for status values. THe interpreter will always turn a variable returned into a single byte integer value.
* Return values in Bash are meant as status values. `0` means "ran okay" and anything else means abnormal in someway.
* Bash is text focused, programs and scripts produce output to be consumed/captured by others, so that's how Bash expects functions to communicate to the caller.
* Function writes data to `stdout` caller captures all `stdout` produced by function then parses/extracts what it wants.

```
function sum()
{
	local first=$1
	local second=$2
	local total=$((first + second))
	echo "${total}"
}

result=$(sum 10 20)  # $(command) captures output of command in single line
```

# Pattern Matching
Use a regular expression to describe a text pattern we expected and then see if given strings do or do not match that pattern.

Regular expressions describe fragments of strings, sequences of characters, and ways to combine them to create more complex strings.
* An integer (as text).
* Is one ore more digits ("0" to "9") in a row with no other characters.

## Bash Regex Syntax
You put patterns in strings:
```
pattern="foo"  # pattern we want to check for
read -p "enter some text" text  # user entered text

if [[ ${text} ~= ${pattern} ]] ; then
	echo "${text} contains ${pattern}"
fi
```

## Patterns
* Any text I want to appear exactly
```
pattern='foo|blah'  # contains either foo or blah

(patternA)|(patternB)  # brackets to group complex patterns

(pattern)*  # The pattern can repeat 0 or more times

(pattern)+  # one or more repetitions

(pattern){m,n}  # m to n repetitions

(pattern)?  # 0 or 1 repetitions

[aeiou]  # any one character

[a-z]  # any character in range a to z (uses ASCII)

[^aeiou]  # any character except the ones listed

[^a-z]  # any character not in the range a to z

if [[ $text~=$pattern ]] ; then
	echo "text contains pattern"
fi
```

To test exact matches we can specify patterns relative to the end or beginning of a string:
```
^  # when outside [] means beginning of a string
$  # means the end of the string
pattern"^foo$"  # foo must be at the start of the string and at the end of the string; therefore, text must only be "foo"  
```

#### Pattern: Get Integer from User
Format: 
* optional +/-
* no leading zeroes
* no trailing zeroes

```
pattern=`^[+-]?[1-9][0-9]*[.][0-9]*[1-9]$'
```
**NOTE:** This does not accept: 9.0, 0.5
```
^[+-]?([0]|[1-9][0-9]*)[.]([0]|[0-9]*[1-9])$
```
#### Pattern: Double Quotes


A pattern to match strings that begin and end with a double quote and do not have a double quote inside
```
pattern='^\"[^\"]*\"$'
```

#### Pattern for Time
```
# hh:mm:ss  ==>  0-12 : 0-59 : 0-59
	
pattern="^([0][1-9]|[1][0-2])[:][0-5][0-9]{2}$"
	
```

#### Match Positive Integer
```
pattern="^[0-9]+$"
```

# Bash and Recursion
Recursively explore the contents of a directory

```
#! /bin/bash
dirname="foo/blah"
# print contents one item at a time
for item in "${dirname}/"* ; do  # will make a list of directory contents
	echo "found ${item}"
done
```

```
function explore()
{
	if [ $# -new 1 ] ; then
		echo "wrong number of parameters"
	else
		local dir=$1
		echo "found ${dir}"
		if [ -d ${dir} ] ; then
			for item in "${dir}/"* ; do
				echo "found ${item}"
				if [ -d ${item} ] ; then
					explore ${item}
				fi
			done
		fi
	fi
}
```
