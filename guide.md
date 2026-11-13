# Bash Conditionals: if, else, elif and case

In many scripts that you will write, there will be, at one point, a need to take decisions based on _something_. Whether it's an argument, a variable value, an exit code, etc. To do that, you'll need to use _conditionals_.

Conditionals, as you may deduce from the name, let you take actions based on one (or more!) conditions. In this guide I will show you how to use them properly in a script. 

There's 4 keywords for conditionals in Bash: `if`, `else`, `elif`, and `case`. We will go over here in the following sections. If you just need a quick reference table, scroll [to the end of the guide](#reference-tableconclusion)!


## If statements

`if` will be the first keyword we will go over.

The basis of an if statement is the following:
```bash
if [[ conditions ]]; then
    commands
fi
```
`if` checks if the answer to an operation is true (exit code 0), and if it is it will execute the commands that come after `then` and before `fi`.

>**NOTE**: _Technically speaking_, using the double square brackets isn't POSIX compliant (POSIX is a norm for Unix software compatibility), so if you want to be fully POSIX compliant you should use only one square bracket, but that make the statement behave differently. If you're interested reading more, I'd suggest reading [Baeldung's post on the matter](https://www.baeldung.com/linux/bash-single-vs-double-brackets). In this guide I'll be using double square bracket thought.

Take caution to the whitespace after the first pair of square brackets and before the last one. It is mandatory, if you don't put it the script may fail.

There's a very wide variety of conditionals you can use. I made this little table to help you get a brief understanding of them:

| Operator | What does it actually do | Example
| ----- | ----- | ----- |
| `-eq` | Verifies if the left operand is equal to the right's **(this is for numbers only)** | `[[ 1 -eq 1 ]]`, returns `true` (so the commands are executed)
| `-ne` | Verifies if the left operand is not equal to the right's **(this is for numbers only)** | `[[ 1 -ne 0 ]]`, returns `true` (so the commands are executed)
| `-z` | Verifies if the string to the right is empty | `[[ -z "A string" ]]`, returns `false` because `"A string"` isn't empty (so the commands are not executed)
| `-n` | Verifies if the string to the right is not empty | `[[ -n "A string" ]]`, returns `true` because `"A string"` isn't empty (so the commands are executed)
| `==` | Verifies if the string to the left is equal to the one on the right **(This is `-eq` equivalent but for strings only)** | `[[ "yes" == "no" ]]`, returns `false` because `"yes"` is not equal to `"no"` (so the commands are not executed)
| `!=` | Verifies if the string to the left is not equal to the one on the right **(This is `-ne` equivalent but for strings only)** | `[[ "yes" != "no" ]]`,returns `true` because `"yes"` is not equal to `"no"` (so the commands are executed)
| `-lt` | Verifies if the number to the left is less than the one on the right | `[[ 1 -lt 2 ]]`, returns `true` because 1 is lower than 2 (so the commands are executed)
| `-le` | Verifies if the number to the left is less or equal to the one on the right | `[[ 1 -le 1 ]]`, returns `true` because 1 is lower or equal to 1 (so the commands are executed)
| `-gt` | Verifies if the number on the left is greater than the one on the right | `[[ 5 -gt 10 ]]`, returns `false` because 5 is not greater than 10 (so the commands are not executed)
| `-ge` | Verifies if the number on the left is greater or equal than the one on the right | `[[ 5 -ge 4 ]]`, returns `true` because 5 is greater or equal to 4 (so the commands are executed)

## Else statements
`else` lets you do another action if an `if` isn't true. For example:
```bash
if [[ $variable -eq 5 ]]; then
    echo "variable is at 5"
else
    echo "variable is not at 5"
fi
```
This example checks if the variable called `$variable` is equal to 5. If it is, it prints `variable is at 5`. If `$variable` is _not_ equal to 5, then `variable is not at 5` gets printed.


`else` statements **must** follow an if statement; you can't use one without an if preceding it. Also, `else` cannot have conditionals (for example, you cannot do "if X isn't true, but Y is" using `else`. For that you need to yse [elif statements](#elif-statements) ).

## Elif statements
`elif` statements literally stands for "else if": it's like an `else` statement, but that can `if` conditionals. For example, let say you want to check if `$variable` is at 5. If it is, you want to print `variable is at 5`. If it is _not_ at 5, but it is under 10, you want to print `variable not at 5 but under 10`. If neither is true, then you want to print `variable neither 5 nor under 10`. Here's how you'd use `elif` to do so:

```bash
if [[ $variable -eq 5 ]]; then
    echo "variable is at 5"
elif [[ $variable -lt 10 ]]; then
    echo "variable not at 5 but under 10"
else
    echo "variable neither 5 nor under 10"
fi
```
Of course you can chain more than one `elif`, so you could have, for example, one `if`, 3 `elif`s, and one `else` at the end to catch everything the `elif`s doesn't.

## Case

Now let say you need to check _a lot of things_ and you don't want to make a nested `if`s structure because it'd be frankly annoying. `case` exists just for that reason: it let you execute different commands depending on the value of a specific event. Here's an example: you take user input in a variable called `input`, and you want to have commands executed depending on what the input it (`greet`, `help`) and return a list of accepted keywords if the input is not one of these. Here's how to do it with `case`:
```bash
case $input in

    greet)
        echo "Hello"
        ;;
    help)
        echo "This is a demo to show you case"
        ;;
    *)
        echo "Unknown keyword, accepted one are: greet and help."
        ;;
    
esac

```
Here, we can see that we have possible cases: `$input` is either `greet`, `help` or anything other than these two (symbolized by the `*`). 

You can make more complex cases. For example, to catch both `yes` and `y`, you can do:
```bash
<snip>
    y | yes)
        <commands>
        ;;
<snip>
```

## Reference Table/Conclusion