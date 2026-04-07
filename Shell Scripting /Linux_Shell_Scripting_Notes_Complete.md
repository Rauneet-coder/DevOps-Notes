# Linux — Shell Scripting
> Short & clear revision notes · 18 topics

---

## 01 · What is a Shell?

A **shell** is an environment that provides an interface to the Unix system. It gathers input from you and executes programs based on that input. When a program finishes executing, it displays the output.

### Shell Types

There are two major types of shells in Unix:

| Type | Default Prompt |
|------|---------------|
| Bourne shell | `$` |
| C shell | `%` |

**Bourne shell subcategories:**

| Shell | Name |
|-------|------|
| `sh` | Bourne Shell |
| `ksh` | Korn Shell |
| `bash` | GNU Bourne-Again Shell (most common) |
| `sh` | POSIX Shell |

**C-type shells:**

| Shell | Name |
|-------|------|
| `csh` | C Shell |
| `tcsh` | TENEX/TOPS C Shell |

> 💡 The original Unix shell was written in the mid-1970s by **Stephen R. Bourne** at AT&T Bell Labs. It is usually installed as `/bin/sh` on most Unix versions.

### Shell Prompt

The `$` prompt is issued by the shell. You type a command after it and press Enter — the shell reads and executes it.

```bash
$date
Thu Jun 25 08:30:19 MST 2009
```

---

## 02 · What is a Shell Script?

A **shell script** is a plain text file containing a list of commands that are listed and executed in order. A good shell script will have comments (preceded by `#`) describing each step.

Shell scripts and functions are **interpreted** — they are not compiled.

### Creating and running a script

```bash
# 1. Create the file
vi test.sh

# 2. Make it executable
chmod +x test.sh

# 3. Run it
./test.sh
```

### The Shebang line

Every script should start with a **shebang** — it tells the system which shell to use:

```bash
#!/bin/sh       # use Bourne shell
#!/bin/bash     # use bash (most common choice)
```

> 💡 The `#` symbol is called a **hash** and `!` is called a **bang** — together they form the "shebang".

### Shell Comments

```bash
#!/bin/bash
# Author : Zara Ali
# Copyright (c) Tutorialspoint.com
# Script follows here:
pwd
ls
```

### Reading user input

```bash
#!/bin/sh
echo "What is your name?"
read PERSON
echo "Hello, $PERSON"
```

---

## 03 · Variables

### Variable Naming Rules

Variable names can only contain **letters** (a–z, A–Z), **numbers** (0–9), and **underscores** (`_`). By convention, shell variable names are in **UPPERCASE**.

**Valid names:** `_ALI`, `TOKEN_A`, `VAR_1`

**Invalid names:** `2_VAR`, `-VARIABLE`, `VAR1-VAR2`, `VAR_A!`

### Defining and Accessing Variables

```bash
NAME="Zara Ali"     # set a variable (no spaces around =)
echo $NAME          # access with $ prefix
echo "Hello, $NAME" # use inside a string
```

### Reading input from the user

```bash
echo "What is your name?"
read PERSON
echo "Hello, $PERSON"
```

### Read-only Variables

```bash
readonly NAME="Zara Ali"
NAME="Qadiri"   # Error: This variable is read only
```

### Unsetting a Variable

```bash
unset NAME      # removes the variable; cannot unset readonly variables
echo $NAME      # prints nothing
```

### Variable Types

| Type | Description |
|------|-------------|
| **Local Variables** | Present only in the current shell instance; set at command prompt |
| **Environment Variables** | Available to any child process of the shell |
| **Shell Variables** | Special variables set by the shell itself to function correctly |

---

## 04 · Special Variables

These variables are reserved for specific functions in the shell:

| Variable | Description |
|----------|-------------|
| `$0` | Filename of the current script |
| `$1`, `$2` ... | Command-line arguments (positional parameters) |
| `$#` | Total number of arguments passed |
| `$*` | All arguments as a single double-quoted string |
| `$@` | All arguments as individually double-quoted strings |
| `$?` | Exit status of the last command (`0` = success) |
| `$$` | PID of the current shell / script |
| `$!` | Process number of the last background command |

```bash
# Example: ./script.sh Zara Ali
echo $0    # ./script.sh
echo $1    # Zara
echo $2    # Ali
echo $#    # 2
```

### `$*` vs `$@`

Both refer to all command-line arguments — the difference shows when enclosed in double quotes:

- `"$*"` — treats the entire list as **one argument** with spaces between
- `"$@"` — separates the list into **individual arguments**

```bash
for TOKEN in $*
do
  echo $TOKEN
done
# Output: Zara Ali 10 Years Old (each on new line)
```

### Exit Status

```bash
$echo $?   # prints 0 if last command was successful, 1 if not
```

> 💡 Most commands return `0` on success and `1` on failure. Some return additional values for specific error types.

---

## 05 · Quotes & Escaping

### Quoting Types

| Type | Syntax | Effect |
|------|--------|--------|
| Single quotes | `'...'` | Everything inside is **literal** — no expansion at all |
| Double quotes | `"..."` | Variables and commands inside **are expanded** |
| Backslash | `\` | Escapes the **next single character** |
| Back quotes | `` `command` `` | Runs a command and uses its output |

```bash
NAME="Zara"
echo '$NAME'          # $NAME  (literal — no expansion)
echo "$NAME"          # Zara   (variable expanded)
echo "Today: `date`"  # Today: Tue Apr 07 2026
echo "I have \$1200"  # I have $1200  (escaped $ sign)
```

### Metacharacters

These characters have special meaning in the shell and must be quoted if you want them treated literally:

```
* ? [ ] ' " \ $ ; & ( ) | ^ < > newline space tab
```

### Single Quotes — quoting large groups

```bash
echo '<-$1500.**>; (update?) [y|n]'   # prints everything literally
```

### Double Quotes — what still works inside

Inside double quotes, the following still have special meaning:
- `$` — parameter substitution
- `` ` `` — command substitution
- `\$`, `\"`, `\\`, `` \` `` — escaped literal characters

### Back Quotes — command substitution

```bash
DATE=`date`
echo "Current Date: $DATE"
```

---

## 06 · Shell Substitution

### Variable Substitution with `echo -e`

The `-e` option enables interpretation of backslash escape sequences:

```bash
a=10
echo -e "Value of a is $a \n"   # prints value then newline
echo    "Value of a is $a \n"   # prints \n literally
```

### Escape Sequences (used with `echo -e`)

| Escape | Description |
|--------|-------------|
| `\\` | Backslash |
| `\a` | Alert (BEL) |
| `\b` | Backspace |
| `\c` | Suppress trailing newline |
| `\f` | Form feed |
| `\n` | New line |
| `\r` | Carriage return |
| `\t` | Horizontal tab |
| `\v` | Vertical tab |

### Command Substitution

The shell runs the command inside backticks and replaces it with the output:

```bash
DATE=`date`
echo "Date is $DATE"

USERS=`who | wc -l`
echo "Logged in users: $USERS"

UP=`date ; uptime`
echo "Uptime is $UP"
```

### Variable Substitution Forms

| Form | Description |
|------|-------------|
| `${var}` | Substitutes the value of `var` |
| `${var:-word}` | If `var` is null/unset, use `word` (var unchanged) |
| `${var:=word}` | If `var` is null/unset, set `var` to `word` |
| `${var:?message}` | If `var` is null/unset, print `message` to stderr |
| `${var:+word}` | If `var` is set, use `word` (var unchanged) |

```bash
echo ${var:-"Variable is not set"}   # uses default, var unchanged
echo ${var:="Variable is not set"}   # sets var to default
echo ${var:?"Print this message"}    # error if var is unset
```

---

## 07 · Basic Operators

### Arithmetic Operators

Use `expr` for arithmetic (spaces required around operators, `*` must be escaped):

```bash
a=10; b=20
echo `expr $a + $b`     # 30
echo `expr $a - $b`     # -10
echo `expr $a \* $b`    # 200  (escape * with \)
echo `expr $b / $a`     # 2
echo `expr $b % $a`     # 0
```

Or use double parentheses (bash):

```bash
echo $((a + b))    # 30
echo $((a * b))    # 200
```

> ⚠️ Spaces are mandatory around operators in `expr`. `2+2` is wrong — use `2 + 2`.

### Relational Operators (for numbers)

| Operator | Meaning | Example |
|----------|---------|---------|
| `-eq` | Equal to | `[ $a -eq $b ]` |
| `-ne` | Not equal to | `[ $a -ne $b ]` |
| `-gt` | Greater than | `[ $a -gt $b ]` |
| `-lt` | Less than | `[ $a -lt $b ]` |
| `-ge` | Greater than or equal | `[ $a -ge $b ]` |
| `-le` | Less than or equal | `[ $a -le $b ]` |

### Boolean Operators

| Operator | Meaning | Example |
|----------|---------|---------|
| `!` | Logical NOT | `[ ! false ]` is true |
| `-o` | Logical OR | `[ $a -lt 20 -o $b -gt 100 ]` |
| `-a` | Logical AND | `[ $a -lt 20 -a $b -gt 100 ]` |

### String Operators

| Operator | Meaning | Example |
|----------|---------|---------|
| `=` | Strings are equal | `[ $a = $b ]` |
| `!=` | Strings are not equal | `[ $a != $b ]` |
| `-z` | String is empty (zero length) | `[ -z $a ]` |
| `-n` | String is not empty | `[ -n $a ]` |
| `str` | String is not empty | `[ $a ]` |

### File Test Operators

| Operator | Meaning |
|----------|---------|
| `-f file` | File exists and is a regular file |
| `-d file` | File exists and is a directory |
| `-e file` | File exists (any type) |
| `-r file` | File is readable |
| `-w file` | File is writable |
| `-x file` | File is executable |
| `-s file` | File exists and is not empty |
| `-b file` | File is a block special file |
| `-c file` | File is a character special file |
| `-g file` | File has SGID bit set |
| `-k file` | File has sticky bit set |
| `-p file` | File is a named pipe |
| `-u file` | File has SUID bit set |

> ⚠️ Always put spaces inside `[ ]` — `[ $a -gt $b ]` not `[$a -gt $b]`.

---

## 08 · Decision Making — `if`, `elif`, `else`

Unix Shell supports these forms of `if` statement: `if...fi`, `if...else...fi`, `if...elif...else...fi`.

### Syntax

```bash
if [ condition ]; then
    # commands
elif [ other_condition ]; then
    # commands
else
    # commands
fi
```

### Examples

```bash
# Compare numbers
if [ $a -gt $b ]; then
    echo "$a is greater than $b"
fi

# Check if a file exists
if [ -f "/etc/passwd" ]; then
    echo "File exists"
else
    echo "File not found"
fi

# Check string
if [ "$NAME" = "Zara" ]; then
    echo "Welcome back!"
fi
```

### `case...esac` Statement — cleaner than multiple `elif`

Similar to `switch...case` in C/C++. Used when all branches depend on the value of a single variable.

```bash
case $DAY in
    Monday)
        echo "Start of the week" ;;
    Friday)
        echo "Almost weekend" ;;
    Saturday | Sunday)
        echo "Weekend!" ;;
    *)
        echo "Midweek" ;;
esac
```

---

## 09 · Loops

### `while` loop — runs while condition is true

```bash
COUNT=1
while [ $COUNT -le 5 ]; do
    echo "Count: $COUNT"
    COUNT=$((COUNT + 1))
done
```

### `for` loop

```bash
# Loop over a list
for VAR in value1 value2 value3; do
    echo $VAR
done

# Loop over files
for file in *.txt; do
    echo "Processing $file"
done

# Loop with a range (bash)
for i in {1..5}; do
    echo "Number $i"
done
```

### `until` loop — runs until condition becomes true (opposite of while)

```bash
COUNT=1
until [ $COUNT -gt 5 ]; do
    echo "Count: $COUNT"
    COUNT=$((COUNT + 1))
done
```

### `select` loop — for creating menus

Creates a numbered menu from a list and prompts the user to select an item.

```bash
select OPTION in "option1" "option2" "quit"; do
    echo "You selected: $OPTION"
    [ "$OPTION" = "quit" ] && break
done
```

### Nested Loops

All loops support nesting. Example — nested `while`:

```bash
#!/bin/sh
a=0
while [ "$a" -lt 10 ]   # outer loop
do
    b="$a"
    while [ "$b" -ge 0 ]  # inner loop
    do
        echo -n "$b "
        b=`expr $b - 1`
    done
    echo
    a=`expr $a + 1`
done
```

> 💡 `echo -n` suppresses the trailing newline.

---

## 10 · Loop Control

### The `break` Statement

Terminates the entire loop immediately and jumps to the code after the loop.

```bash
a=0
while [ $a -lt 10 ]; do
    echo $a
    if [ $a -eq 5 ]; then
        break        # stops at 5
    fi
    a=`expr $a + 1`
done
```

**Breaking out of nested loops:**

```bash
break n    # exits the nth enclosing loop
```

```bash
for var1 in 1 2 3; do
    for var2 in 0 5; do
        if [ $var1 -eq 2 -a $var2 -eq 0 ]; then
            break 2    # exits BOTH loops
        else
            echo "$var1 $var2"
        fi
    done
done
```

### The `continue` Statement

Skips the rest of the current iteration and moves to the next one.

```bash
NUMS="1 2 3 4 5 6 7"
for NUM in $NUMS; do
    Q=`expr $NUM % 2`
    if [ $Q -eq 0 ]; then
        echo "Even number"
        continue      # skip rest, go to next iteration
    fi
    echo "Odd number"
done
```

**Skipping in nested loops:**

```bash
continue n    # continues from the nth enclosing loop
```

### Infinite Loop

A loop runs forever if its condition is never met:

```bash
a=10
until [ $a -lt 10 ]; do   # condition never true → infinite loop
    echo $a
    a=`expr $a + 1`
done
```

### Loop Control Summary

| Command | What it does |
|---------|--------------|
| `break` | Exit the loop immediately |
| `break n` | Exit the nth enclosing loop |
| `continue` | Skip the rest of this iteration, go to next |
| `continue n` | Continue from the nth enclosing loop |

---

## 11 · Functions

Functions let you break a script into smaller, reusable logical subsections — similar to subroutines in other languages.

### Defining and Calling a Function

```bash
function_name () {
    list of commands
}

# Call it
function_name
```

```bash
Hello () {
    echo "Hello World"
}
Hello    # Hello World
```

### Passing Parameters to a Function

Parameters are accessed as `$1`, `$2`, etc. inside the function:

```bash
Hello () {
    echo "Hello World $1 $2"
}
Hello Zara Ali    # Hello World Zara Ali
```

### Returning Values from Functions

```bash
# Using return (returns a numeric exit code, captured via $?)
Hello () {
    echo "Hello $1 $2"
    return 10
}
Hello Zara Ali
echo "Return value: $?"    # Return value: 10

# Using echo to return actual data
add() {
    result=$(($1 + $2))
    echo $result
}
SUM=$(add 5 3)
echo "Sum is $SUM"         # Sum is 8
```

> 💡 `return` only returns a numeric exit code (0–255). To return actual values, use `echo` and capture with `$()`.

### Nested Functions

Functions can call each other:

```bash
number_one () {
    echo "This is the first function speaking..."
    number_two
}
number_two () {
    echo "This is now the second function speaking..."
}
number_one
```

### Removing a Function Definition

```bash
unset -f function_name
```

### Function Call from Prompt

Put function definitions in `.profile` to make them available at every login. Or source a file:

```bash
$. test.sh         # load functions from test.sh into current shell
$ number_one       # now callable directly
```

---

## 12 · Arrays

A **scalar variable** holds one value. An **array variable** can hold multiple values at the same time.

### Defining Arrays

```bash
# Method 1: assign by index
NAME[0]="Zara"
NAME[1]="Qadir"
NAME[2]="Mahnaz"
NAME[3]="Ayan"
NAME[4]="Daisy"

# Method 2: bash syntax (all at once)
NAME=(Zara Qadir Mahnaz Ayan Daisy)

# ksh syntax
set -A NAME Zara Qadir Mahnaz Ayan Daisy
```

### Accessing Array Values

```bash
echo ${NAME[0]}    # Zara     (first element)
echo ${NAME[1]}    # Qadir    (second element)

# Access ALL elements
echo ${NAME[*]}    # Zara Qadir Mahnaz Ayan Daisy
echo ${NAME[@]}    # Zara Qadir Mahnaz Ayan Daisy
```

---

## 13 · Input, Output & Redirection

Most Unix commands read from **standard input** (keyboard) and write to **standard output** (terminal) by default. Redirection lets you change these sources/destinations.

### Redirection Operators

| Operator | What it does |
|----------|--------------|
| `>` | Redirect output to a file (overwrites) |
| `>>` | Append output to a file |
| `<` | Take input from a file |
| `2>` | Redirect error output (STDERR) to a file |
| `2>&1` | Redirect STDERR to same place as STDOUT |
| `\|` | Pipe output of one command to another |
| `n > file` | Redirect stream with descriptor `n` to file |
| `n >> file` | Append stream `n` to file |
| `n >& m` | Merge output stream `n` with stream `m` |
| `n <& m` | Merge input stream `n` with stream `m` |

> 💡 File descriptor: `0` = STDIN, `1` = STDOUT, `2` = STDERR

```bash
ls -l > filelist.txt          # save output to file (overwrites)
ls -l >> filelist.txt         # append to file
sort < filelist.txt           # sort reads from file
ls /fake 2> errors.txt        # save error messages to file
command > /dev/null 2>&1      # discard ALL output (both stdout & stderr)
echo message 1>&2             # display a message on STDERR
```

### Here Document

Feeds multiple lines of input directly into a command without needing a separate file:

```bash
command << DELIMITER
line 1
line 2
line 3
DELIMITER
```

```bash
# Count lines with here document
wc -l << EOF
    This is a simple lookup program
    for good restaurants
    in Cape Town.
EOF
# Output: 3

# Print multiple lines
cat << EOF
This is a simple lookup program
for good restaurants
in Cape Town.
EOF
```

> ⚠️ The delimiter must be a **single word** with no spaces or tabs. It must appear alone on the closing line.

### Discard Output

```bash
command > /dev/null        # discard stdout only
command > /dev/null 2>&1   # discard both stdout and stderr
```

---

## 14 · Man Pages (Help System)

Unix's built-in help system. Every command has a **man page** (manual page).

### Syntax

```bash
man command
man pwd      # get help on the pwd command
man man      # get help on man itself
```

### Man Page Sections

| Section | Description |
|---------|-------------|
| NAME | Name of the command |
| SYNOPSIS | General usage and parameters |
| DESCRIPTION | What the command does |
| OPTIONS | All arguments/options |
| SEE ALSO | Related commands |
| BUGS | Known issues |
| EXAMPLES | Common usage examples |
| AUTHORS | Author of the man page |

> 💡 Man pages are the **first place to look** when you don't know how to use a command.

---

## Quick Reference

```
Create & run:
  chmod +x script.sh
  ./script.sh arg1 arg2

Variables:
  NAME="value"          # set
  echo $NAME            # use
  read VAR              # get input
  readonly VAR          # make read-only
  unset VAR             # delete

Special vars:
  $1 $2 ...   arguments
  $#          argument count
  $?          last exit status (0=ok)
  $$          script PID
  $!          last background PID

Quotes:
  'literal'             # no expansion
  "expands $VAR"        # expansion allowed
  `command`             # command substitution
  \char                 # escape one character

Substitution:
  ${var:-default}       # use default if unset
  ${var:=default}       # set to default if unset
  ${var:?message}       # error if unset

Operators:
  expr $a + $b          # arithmetic with expr
  $((a + b))            # arithmetic with (())
  [ $a -eq $b ]         # numbers equal
  [ "$s" = "hi" ]       # strings equal
  [ -f file ]           # file exists
  [ -d dir ]            # dir exists
  [ -z "$s" ]           # string is empty
  ! / -o / -a           # NOT / OR / AND

Conditions:
  if [ cond ]; then ... elif ... else ... fi
  case $VAR in val) ... ;; esac

Loops:
  for x in a b c; do ... done
  while [ cond ]; do ... done
  until [ cond ]; do ... done
  break / break n / continue / continue n

Arrays:
  ARR=(a b c)           # define
  ${ARR[0]}             # access by index
  ${ARR[*]}             # all elements

Functions:
  myfunc() { echo $1; }
  myfunc "hello"
  ret=$?                # capture return code
  unset -f myfunc       # remove function

Redirect:
  cmd > file            # save output (overwrite)
  cmd >> file           # append output
  cmd < file            # read input from file
  cmd 2> file           # save errors
  cmd > /dev/null 2>&1  # discard everything
  cmd1 | cmd2           # pipe
  cmd << EOF ... EOF    # here document

Help:
  man command           # open manual page
```

---

*Linux Shell Scripting Notes — for revision use*
