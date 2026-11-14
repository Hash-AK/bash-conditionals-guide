# Bash Conditionals: if, else, elif, and case

In many scripts that you will write, there will be, at one point, a need to make decisions based on _something_. Whether it's an argument, a variable value, an exit code, etc. To do that, you'll need to use _conditionals_.

Conditionals, as you may deduce from the name, let you take actions based on one (or more!) conditions. In this guide I will show you how to use them properly in a script. 

There are four keywords for conditionals in Bash: `if`, `else`, `elif`, and `case`. We will go over here in the following sections. If you just need a quick reference table, scroll [to the end of the guide](#reference-tableconclusion)!


## If statements

`if` will be the first keyword we will go over.

The basis of an if statement is the following:
```bash
if [[ conditions ]]; then
    commands
fi
```
`if` checks if the answer to an operation is true (exit code 0), and if it is it will execute the commands that come after `then` and before `fi`.

>**NOTE**: _Technically speaking_, using the double square brackets isn't POSIX compliant (POSIX is a norm for Unix software compatibility), so if you want to be fully POSIX compliant you should use only one square bracket, but that makes the statement behave differently. If you're interested reading more, I'd suggest reading [Baeldung's post on the matter](https://www.baeldung.com/linux/bash-single-vs-double-brackets). In this guide I'll be using double square bracket though.

Take caution to the whitespace after the first pair of square brackets and before the last one. It is mandatory, if you don't put it the script may fail.

There's a very wide variety of operators you can use. I made this little table to help you get a brief understanding of them:

| Operator | What does it actually do | Example
| ----- | ----- | ----- |
| `-eq` | Verifies if the left operand is equal to the right's **(this is for numbers only)** | `[[ 1 -eq 1 ]]`, returns `true` (so the commands are executed)
| `-ne` | Verifies if the left operand is not equal to the right's **(this is for numbers only)** | `[[ 1 -ne 0 ]]`, returns `true` (so the commands are executed)
| `-z` | Verifies if the string to the right is empty | `[[ -z "A string" ]]`, returns `false` because `"A string"` isn't empty (so the commands are not executed)
| `-n` | Verifies if the string to the right is not empty | `[[ -n "A string" ]]`, returns `true` because `"A string"` isn't empty (so the commands are executed)
| `==` | Verifies if the string to the left is equal to the one on the right **(This is `-eq` equivalent but for strings only)** | `[[ "yes" == "no" ]]`, returns `false` because `"yes"` is not equal to `"no"` (so the commands are not executed)
| `!=` | Verifies if the string to the left is not equal to the one on the right **(This is `-ne` equivalent but for strings only)** | `[[ "yes" != "no" ]]`,returns `true` because `"yes"` is not equal to `"no"` (so the commands are executed)
| `-lt` | Verifies if the number to the left is less than the one on the right | `[[ 1 -lt 2 ]]`, returns `true` because 1 is lower than 2 (so the commands are executed)
| `-le` | Verifies if the number to the left is less than or equal to the one on the right | `[[ 1 -le 1 ]]`, returns `true` because 1 is lower than or equal to 1 (so the commands are executed)
| `-gt` | Verifies if the number on the left is greater than the one on the right | `[[ 5 -gt 10 ]]`, returns `false` because 5 is not greater than 10 (so the commands are not executed)
| `-ge` | Verifies if the number on the left is greater than or equal than the one on the right | `[[ 5 -ge 4 ]]`, returns `true` because 5 is greater than or equal to 4 (so the commands are executed)
| `-e` | Verifies if the file path on the right exists | `[[ -e /home/user1/.bashrc ]]`
| `-r` | Verifies if the file path on the right is readable | `[[ -r /etc/passwd ]]`
| `-w` | Verifies if the file path on the right is writable | `[[ -w /home/user1/a-random-file.txt ]]`
| `-x` | Verifies if the file path on the right is executable | `[[ -x /bin/ls ]]`
| `-s` | Verifies if the file path on the right is not empty (size is greater than 0) | `[[ -s /var/log/a-log ]]`
| `-d` | Verifies if the file path on the right is a directory | `[[ -d /home/ ]]`
| `-f` | Verifies if the file path on the right is a file | `[[ -f /home/user1/notes.txt ]]`

### Combining operators

You can combine more than one operator when doing an `if` (or `elif`, as we will see later) statement. Here's a few examples:

```bash
if [[ -r /home/user1/a-random-file.txt && -w /home/user1/a-random-file.txt ]]; then
    echo "file is readable and writable"
fi
```
As we can see here, you can combine two conditions using `&&`, meaning "if condition A **and** condition B are true, then...".

```bash
if [[ -r /home/user1/a-file.txt || -r /home/user1/another-file.txt ]]; then
    echo "one of the two files is readable"
fi
```
Here we do "if **either** condition A _or_ condition B is true, then...", using `||`.

## Else statements
`else` lets you do another action if an `if` isn't true. For example:
```bash
if [[ "$variable" -eq 5 ]]; then
    echo "variable is at 5"
else
    echo "variable is not at 5"
fi
```
This example checks if the variable called `$variable` is equal to 5. If it is, it prints `variable is at 5`. If `$variable` is _not_ equal to 5, then `variable is not at 5` gets printed.


`else` statements **must** follow an if statement; you can't use one without an if preceding it. Also, `else` cannot have conditionals (for example, you cannot do "if X isn't true, but Y is" using `else`. For that you need to use [elif statements](#elif-statements) ).

## Elif statements
An `elif` statement literally stands for "else if": it's like an `else` statement, but that can `if` conditionals. For example, let's say you want to check if `$variable` is at 5. If it is, you want to print `variable is at 5`. If it is _not_ at 5, but it is under 10, you want to print `variable not at 5 but under 10`. If neither is true, then you want to print `variable neither 5 nor under 10`. Here's how you'd use `elif` to do so:

```bash
if [[ "$variable" -eq 5 ]]; then
    echo "variable is at 5"
elif [[ "$variable" -lt 10 ]]; then
    echo "variable not at 5 but under 10"
else
    echo "variable neither 5 nor under 10"
fi
```
Of course you can chain more than one `elif`, so you could have, for example, one `if`, 3 `elif`s, and one `else` at the end to catch everything the `elif`s don't.

## Case

Now let's say you need to check _a lot of things_ and you don't want to make a nested `if`s structure because it'd be frankly annoying. `case` exists just for that reason: it lets you execute different commands depending on the value of a specific event. Here's an example: you take user input in a variable called `input`, and you want to have commands executed depending on what the input is (`greet`, `help`) and return a list of accepted keywords if the input is not one of these. Here's how to do it with `case`:
```bash
case "$input" in

    greet)
        echo "Hello"
        ;;
    help)
        echo "This is a demo to show you case"
        ;;
    *)
        echo "Unknown keyword, accepted ones are: greet and help."
        ;;
    
esac

```
Here, we can see that we have possible cases: `$input` is either `greet`, `help` or anything other than these two (symbolized by the `*`). 

You can make more complex cases. For example, to catch both `yes` and `y`, you can do:
```bash
case "$input" in
    y | yes)
        <commands>
        ;;
    *)
        <commands>
        ;;
esac
```

What if you want the cases to be case insensitive (so that you don't have to catch `y`, `Y`, `yes`, `Yes`, `YES`, `yES`, and so on)? You can use the `nocasematch` shell option to do that! Here's an example:

```bash
# input is the variable that we want to do the case on
shopt -s nocasematch
case "$input" in
    y | yes)
        echo "you said yes"
        ;;
    n | no)
        echo "you said no"
        ;;
    *)
        echo "you said something else"
        ;;
esac
shopt -u nocasematch
```
`shopt` allows us to enable special shell options. In this case, we temporarily enabled the `nocasematch` option, which makes Bash match patterns case-insensitively.

## Exercise: Find the fish!

Now it's time for an exercise! You need to write a script that walks down a directory and its subdirectories and prints every item whose name contains the string "fish".

For that you'll need a loop. If you don't know how to do one, here's the structure:

```bash
for item in **/*; do
    # Your code goes here
done
```

A few hints:

- To make the above loop work, you will need to enable the shell option that makes `**` match subdirectories (`globstar`)
- As you want to match _any_ item that has "fish" in the name, you need to use wildcards before and after that string (so `*fish*` as the condition).
---
### Solution
```bash
#!/bin/bash
shopt -s globstar
for item in **/*; do
    if [[ "$item" == *fish* ]]; then
        echo "Something fishy found: $item"
    fi
done
shopt -u globstar #optional

```
If you want to go further, you can even verify if the item is a directory or a file, and print out which one it is!

## Reference Table/Conclusion

Here's a quick syntax recap:
```bash
if [[ conditions ]]; then
    commands
elif [[ conditions ]]; then
    commands
else
    commands
fi

case "$variable" in
    pattern)
        commands
        ;;
    other)
        commands
        ;;
    *)
        commands
        ;;
esac
```

Use `==`/`!=` for strings, `-eq`/`-ne`/`-lt`/`-le`/`-gt`/`-ge` for numbers,
and `-e`/`-f`/`-d`/`-r`/`-w`/`-x` for files. Quote your variables to prevent surprises.

And that's it! `if`,`else`,`elif`, and `case` covers almost every decisions your script could potentially have to take!