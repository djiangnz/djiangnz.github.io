---
layout: post
title: Bash Cheatsheet
description:
summary:
tags: cheatsheet bash
---

`#!/usr/bin/env bash`

[bash online](https://replit.com/languages/bash)

# [Options](https://www.gnu.org/software/bash/manual/html_node/The-Set-Builtin.html)

```bash
set -o noclobber  # Avoid overlay files (echo "hi" > foo)
set -o errexit    # Used to exit upon error, avoiding cascading errors
set -o pipefail   # Unveils hidden failures
set -o nounset    # Exposes unset variables
set -e            # Exit immediately if a command exits with a non-zero status.
set -x            # Print out commands before executing them.
```

> `+` turn off
>
> `-` turn on

# String quotes

```bash
name="John"
echo "Hi $name"  # Hi John
echo 'Hi $name'  # Hi $name
```

# Variable Types

```bash
a=2334 # Integer
b=BB34 # String
c=''   # null variables (Or c="" Or c=)
```

# Parameter Expansions

## Basics

```bash
name="John"           # If value is not given, the variable is assigned the null string
echo "${name}"
echo "${name/J/j}"    # "john" (substitution)
echo "${name:0:2}"    # "Jo" (slicing)
echo "${name::2}"     # "Jo" (slicing)
echo "${name::-1}"    # "Joh" (slicing)
echo "${name:(-1)}"   # "n" (slicing from right)
echo "${name:(-2):1}" # "h" (slicing from right)
echo "${food:-Cake}"  # $food or "Cake"

length=2
echo "${name:0:length}" # "Jo"

# Indirect expansion:
other_variable="variable"
echo ${!other_variable} # Some string
```

## Default Values

```bash
${foo:-val}     # $foo, or val if unset (or null)
${foo:+val}     # val if $foo is set (and not null)
${foo:=val}     # Set $foo to val if unset (or null)
${foo:?message} # Show error message and exit if $foo is unset (or null)
```

## Substitution

```bash
str="/path/to/foo.cpp"
echo "${str%.cpp}"    # /path/to/foo
echo "${str%.cpp}.o"  # /path/to/foo.o
echo "${str%/*}"      # /path/to

echo "${str##*.}"     # cpp (extension)
echo "${str##*/}"     # foo.cpp (basepath)

echo "${str#*/}"      # path/to/foo.cpp
echo "${str##*/}"     # foo.cpp
echo "${str/foo/bar}" # /path/to/bar.cpp

src="/path/to/foo.cpp"
base=${src##*/}       # "foo.cpp" (basepath)
dir=${src%$base}      # "/path/to/" (dirpath)
```

## Manipulation

```bash
str="HELLO WORLD!"
echo "${str,}"   # "hELLO WORLD!" (lowercase 1st letter)
echo "${str,,}"  # "hello world!" (all lowercase)

str="hello world!"
echo "${str^}"   # "Hello world!" (uppercase 1st letter)
echo "${str^^}"  # "HELLO WORLD!" (all uppercase)
```

# Brace Expansion {...}

```bash
# used to generate arbitrary strings:
echo {1..10} # 1 2 3 4 5 6 7 8 9 10
echo {a..z} # a b c d e f g h i j k l m n
echo {A,B}.js # Same as A.js B.js
```

# Array

```bash
# Defining arrays
Fruits=('Apple' 'Banana' 'Orange')
Fruits[0]="Apple"
Fruits[1]="Banana"
Fruits[2]="Orange"

# Operations
Fruits=("${Fruits[@]}" "Watermelon")    # Push
Fruits+=('Watermelon')                  # Also Push
Fruits=( "${Fruits[@]/Ap*/}" )          # Remove by regex match
unset Fruits[2]                         # Remove one item
Fruits=("${Fruits[@]}")                 # Duplicate
Fruits=("${Fruits[@]}" "${Veggies[@]}") # Concatenate
lines=(`cat "logfile"`)                 # Read from file

# Working with arrays
echo "${Fruits[0]}"           # Element #0
echo "${Fruits[-1]}"          # Last element
echo "${Fruits[@]}"           # All elements, space-separated
echo "${#Fruits[@]}"          # Number of elements
echo "${#Fruits}"             # String length of the 1st element
echo "${#Fruits[3]}"          # String length of the Nth element
echo "${Fruits[@]:3:2}"       # Range (from position 3, length 2)
echo "${!Fruits[@]}"          # Keys of all elements, space-separated

# Iteration
for name in "${arrayName[@]}"; do
  echo "$name"
done
```

# Dictionaries

```bash
# Defining
declare -A sounds
sounds[dog]="bark"
sounds[cow]="moo"
sounds[bird]="tweet"
sounds[wolf]="howl"

# Declares sounds as a Dictionary object (aka associative array).

# Working with dictionaries
echo "${sounds[dog]}" # Dog's sound
echo "${sounds[@]}"   # All values
echo "${!sounds[@]}"  # All keys
echo "${#sounds[@]}"  # Number of elements
unset sounds[dog]     # Delete dog

# Iterate over values
for val in "${sounds[@]}"; do
  echo "$val"
done

# Iterate over keys
for key in "${!sounds[@]}"; do
  echo "$key"
done
```

# Reading Input

```bash
echo -n "Proceed? [y/n]: "
read -r ans
echo "$ans"
# The -r option disables a peculiar legacy behavior with backslashes.

read -n 1 ans    # Just one character
```

# Conditionals

## Conditions

```bash
if [[ "$name" != "$USER" ]]; then
    echo "Your name isn't your username"
else
    echo "Your name is your username"
fi
```

|       Condition         |       Expressions       |
| ------------------------ | --------------------- |
| `[[ -z STRING ]]`        | Empty string          |
| `[[ -n STRING ]]`        | Not empty string      |
| `[[ STRING == STRING ]]` | Equal                 |
| `[[ STRING != STRING ]]` | Not Equal             |
| `[[ NUM -eq NUM ]]`      | Equal                 |
| `[[ NUM -ne NUM ]]`      | Not equal             |
| `[[ NUM -lt NUM ]]`      | Less than             |
| `[[ NUM -le NUM ]]`      | Less than or equal    |
| `[[ NUM -gt NUM ]]`      | Greater than          |
| `[[ NUM -ge NUM ]]`      | Greater than or equal |
| `[[ STRING =~ STRING ]]` | Regex                 |
| `(( NUM < NUM ))`        | Numeric conditions    |
| `[[ ! EXPR ]]`           | Not                   |
| `[[ X && Y ]]`           | And                   |
| `[[ X \|\| Y ]]`         | Or                    |

## File Conditions

|       Condition         |       Expressions       |
| ----------------------- | ----------------------- |
| `[[ -e FILE ]]`         | Exists                  |
| `[[ -r FILE ]]`         | Readable                |
| `[[ -h FILE ]]`         | Symlink                 |
| `[[ -d FILE ]]`         | Directory               |
| `[[ -w FILE ]]`         | Writable                |
| `[[ -s FILE ]]`         | Size is > 0 bytes       |
| `[[ -f FILE ]]`         | File                    |
| `[[ -x FILE ]]`         | Executable              |
| `[[ FILE1 -nt FILE2 ]]` | 1 is more recent than 2 |
| `[[ FILE1 -ot FILE2 ]]` | 2 is more recent than 1 |
| `[[ FILE1 -ef FILE2 ]]` | Same files              |

# Loops

```bash
# Basic forloop
for i in /etc/rc.*; do
  echo "$i"
done

# Reading lines
while read -r line; do
  echo "$line"
done <file.txt

# C-like forloop
for (( i = 0; i < 100; i++ )); do
  echo "$i"
done

# Ranges
for i in {1..10..2}; do
    echo "Welcome $i"
done

# Infinite loops
for (( ; ; )); do
   echo "infinite loops"
done
```

# Functions

```bash
# Defining functions
[function] myfunc() {
    echo "hello $1"
}

# Returning values
myfunc() {
    local myresult='some value'
    echo "$myresult"
}
result=$(myfunc)
```

# [Built-in shell variables](http://linuxsig.org/files/bash_scripting.html)

|Variable|Use|
|---|---|
|`$#`|number of command-line arguments that were passed to the shell program.|
|`$1`|`n`th argument|
|`$?`|exit value of the last command that was executed. 0 means no issues|
|`$0`|name of the shell program. e.g. bash|
|`"$*"`|arguments that were entered on the command line ($1 $2 ...)|
|`"$@"`|arguments that were entered on the command line, individually quoted ("$1" "$2" ...)|
|`$!`|process ID of the job most recently placed into the background|

> - `"$@"` and `"$*"` must be quoted
> - `"$@"` is not bash specific and should work with any POSIX shell
> - `"$*"` expands to the positional parameters, starting from one. When the expansion occurs within **double quotes**, it expands to a single word with the value of each parameter separated by the first character of the `IFS` special variable. That is, `"$*"` is equivalent to  `"$1c$2c..."`, where `c` is the first character of the value of the `IFS` variable. If `IFS` is unset, the parameters are separated by spaces. If `IFS` is null, the parameters are joined without intervening separators.
> - `"$@"` expands to the positional parameters, starting from one. When the expansion occurs within **double quotes**, each parameter expands to a separate word. That is, `"$@"` is equivalent to `"$1" "$2" ...` If the double-quoted expansion occurs within a word, the expansion of the first parameter is joined with the beginning part of the original word, and the expansion of the last parameter is joined with the last part of the original word. When there are no positional parameters, `"$@"` and `$@` expand to nothing (i.e., they are removed).
> - [bash(1) - Linux man page](https://linux.die.net/man/1/bash)

# Miscellaneous

## Numeric calculations

```bash
$((a + 200))      # Add 200 to $a
$(($RANDOM%200))  # Random number 0..199

declare -i count  # Declare as type integer
count+=1          # Increment
```

## Subshells

```bash
(cd somedir; echo "I'm now in $PWD")
pwd # still in first directory
```

## Redirection

```bash
python hello.py > output.txt            # stdout to (file)
python hello.py >> output.txt           # stdout to (file), append
python hello.py 2> error.log            # stderr to (file)
python hello.py 2>&1                    # stderr to stdout
python hello.py 2>/dev/null             # stderr to (null)
python hello.py >output.txt 2>&1        # stdout and stderr to (file), equivalent to &>
python hello.py &>/dev/null             # stdout and stderr to (null)
echo "$0: warning: too many users" >&2  # print diagnostic message to stderr
python hello.py < foo.txt      # feed foo.txt to stdin for python
diff <(ls -r) <(ls)            # Compare two stdout without files
```

## Directory of script

```bash
dir=${0%/*}
```

## Getting options

```bash
while [[ "$1" =~ ^- && ! "$1" == "--" ]]; do case $1 in
  -V | --version )
    echo "$version"
    exit
    ;;
  -s | --string )
    shift; string=$1
    ;;
  -f | --flag )
    flag=1
    ;;
esac; shift; done
if [[ "$1" == '--' ]]; then shift; fi
```
