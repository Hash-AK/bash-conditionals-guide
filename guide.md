# Bash Conditionals: if, else, elif and case

In many scripts that you will write, there will be, at one point, a need to take decisions based on _something_. Whether it's an argument, a variable value, an exit code, etc. To do that, you'll need to use _conditionals_.

Conditionals, as you may deduce from the name, let you take actions based on one (or more!) conditions. In this guide I will show you how to use them properly in a script. 

There's 4 keywords for conditionals in Bash: `if`, `else`, `elif`, and `case`. We will go over here in the following sections. If you just need a quick reference table, scroll [to the end of the guide](#TO ADD WHEN WRITTEN)!


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

| Conditional | What does it actually do | Example
| ----- | ----- | ----- |
| `-eq` | Verifies if the left operand is equal to the right's **(this is for numbers only)** | `[[ 1 -eq 1 ]]`, returns `true` (so the commands are executed)
| `-ne` | Verifies if the left operand is not equal to the right's **(this is for numbers only)** | `[[ 1 -nq 0 ]]`, returns `true` (so the commands are executed)
| `-z` | Verifies if the string to the right is empty | `[[ -z "A string" ]]`, returns `false` because `"A string"` isn't empty (so the commands are not executed)
| `-n` | Verifies if the string to the right is not empty | `[[ -n "A string" ]]`, returns `true` because `"A string"` isn't empty (so the commands are executed)
| `==` | Verifies if the string to the left is equal to the one on the right **(This is `-eq` equivalent but for strings only)** | `[[ "yes" == "no" ]]`, returns `false` because `"yes"` is not equal to `"no"` (so the commands aren't executed)


