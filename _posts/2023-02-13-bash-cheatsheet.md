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
name="John"
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
for i in "${arrayName[@]}"; do
  echo "$i"
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

|                          |                       |
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

|                         |                         |
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
for ((i = 0 ; i < 100 ; i++)); do
  echo "$i"
done

# Ranges
for i in {1..10..2}; do
    echo "Welcome $i"
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

## Arguments

|      |                                                |
| ---- | ---------------------------------------------- |
| `$#` | Number of arguments                            |
| `$*` | All positional arguments (as a single word)    |
| `$@` | All positional arguments (as separate strings) |
| `$1` | First argument                                 |
| `$_` | Last argument of the previous command          |

> `$@` and `$*` must be quoted, See [Special parameters](https://wiki.bash-hackers.org/syntax/shellvars#special_parameters_and_shell_variables)

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
