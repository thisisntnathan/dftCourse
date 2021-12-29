---
layout: page
title: My First Script
sub-title: The DFT Short Course
sidebar_link: true
sidebar_sort_order: 3
permalink: /ShortCourse/firstScript.html
---

Let's write a script in <kbd>bash</kbd>! We'll do this using the command line (with <kbd>vim</kbd>), but feel free to use any text editor.  

Navigate to your home directory and open a new file named `hello.sh`.

```sh
NathanLui@local | ~ $ vim hello.sh
```

In `vim`, type `i` to enter insert mode and type:

```sh
#!/bin/bash

echo "Hello world!"
```

The first line is called the *shebang* (a portmanteau of ha**sh** (#) and **bang** (!)).<sup>1</sup> It tells the operating system where to find the *interpreter* for the program. In this case we are telling the OS that this script can be read and run by <kbd>bash</kbd> which is located at `/bin/bash`. Many different interpreters can be used as an alternative to <kbd>bash</kbd>, for example `#!/bin/python2.7` tells the OS that this script is written in <kbd>python</kbd> and it should be run using an old version of python (2.7) located at `/bin/python2.7`.  

>Recall that <kbd>bash</kbd> is both the shell and the scripting language, so <kbd>bash</kbd> commands we give, also known as the syntax, in the script are executed by the interpreting program `/bin/bash`.

#### Comments

The interpreter doesn't treat this line as a program call since it starts with `#`, the <kbd>bash</kbd> comment symbol. Any text in a <kbd>bash</kbd> script that is preceded with a `#` will be ignored by the interpreter. Note that [different languages have different comment symbols/types](https://en.wikipedia.org/wiki/Comment_(computer_programming)) (e.g. `(* OCaml *)`, `% MATLAB`, `// Java`, `<!-- HTML -->`, etc...). Comments within your code serve two purposes:  
<!-- markdownlint-disable-next-line MD032--> 1) when **other people** read your code they understand your thinking and how you chose to implement the program, and  
<!-- markdownlint-disable-next-line MD032--> 2) when **you** read your code, months or years later, **you** understand your thinking and how you chose to implement the program  

> Comment wide and comment often, but don't comment the obvious!

Now back to our script, press `esc` to back out of insert mode and type `:wq` to **w**rite the file and **q**uit <kbd>vim</kbd>.  If you're doing this in a graphical text editor save the file to the home directory with the name `hello.sh`.  

Now let's look for our new file in the home directory:

```sh
NathanLui@local | ~ $ ls
launchCodes.txt          playGame.sh          Presentation Slides
hello.sh
```

There it is! So lets run it with the command `bash hello.sh`.

```sh
NathanLui@local | ~ $ bash hello.sh
The command was not found or was not executable
```

That's not good!  We're sure the file exists, so it must be our access modes.  Let's check:  

```sh
NathanLui@local | ~ $ ls -l
-rw-r--r--   1 NathanLui  Users    33B Dec 21 13:21 hello.sh
```

There's the issue, not a problem since it's one we already know how to fix.

```sh
NathanLui@local | ~ $ chmod +x hello.sh             # n.b. the +x gives x 
NathanLui@local | ~ $ ls -l                         # permision to everyone
-rwxr-xr-x   1 NathanLui  Users    33B Dec 21 13:21 hello.sh
```

Now our program should run without a hitch.

```sh
NathanLui@local | ~ $ bash hello.sh
Hello world!
```

## <center> 👏👏 You did it! 👏👏 </center>

<!-- <br /> -->

Lets try something a bit more difficult. Open that script back up with `vim hello.sh`, add a variable called `food` and give it a value (like your favorite food):

```sh
#!/bin/bash

echo "Hello world!"
food="pizza"
```

Now let's call that variable with:

```sh
echo "My favorite food is $food."
```

The `$` tells the interpreter that we want the object stored in the variable `food`. Our full script now looks like:  

{% gist 38262bc95d709f20d66237671b94f861 %}

When we run it, the script now prints:

```sh
NathanLui@local | ~ $ bash hello.sh             # no need to change permissions
Hello world!                                    # this time since we did it
My favorite food is pizza.                      # for this file earlier
```

Look at you go! One last thing that we should talk about is an environmental variable. An environmental variable is one whose value is set outside the program. Let's edit our script one more time. Append the line:  

```sh
echo "I am $age years old."
```

So our script is now:  

{% gist da145ae701a9f05e007bb6071843d418 %}

But we haven't declared the `age` variable yet. In some languages this would throw an error, but if we run our program we see:  

```sh
NathanLui@local | ~ $ bash hello.sh
Hello world!
My favorite food is pizza.
I am  years old.
```

This is because <kbd>bash</kbd> automatically initializes uninitialized variables to `null` at first use. So how do we get the script to print our age? We can initialize `age` as an environmental variable and export it to our script. We can do this in one step using `export`.

```sh
NathanLui@local | ~ $ export age=26
NathanLui@local | ~ $ bash hello.sh
Hello world!
My favorite food is pizza.
I am 26 years old.
```

You might be wondering why would ever need to do this. Often times we'll be working with programs that can't be easily modified, or we'll want to set variables once instead of every single time we run the program. These tasks can be easily accomplished using environmental variables.

Scripting is useful for more than telling the world your favorite food and how old you are. Its our primary way of sending instructions to the cluster. When we submit jobs to the CHEM cluster's resource manager <kbd>SLURM</kbd> we do so in the form of shell scripts. More on that in the next chapter.

If you want to read more about the power of scripting I wholely recommend Al Sweigart's book [Automate the Boring Stuff with Python](https://automatetheboringstuff.com/), a fantastic (and free) resource for budding programmers (and busy grad students).<sup>2</sup>  

<br />

| <center>Previous<br><a href="/ShortCourse/linuxBasics.html">Linux Basics</a></center> | <center><a href="/Introduction.html">Home</a></center> | <center>Next<br><a href="/ShortCourse/slurm.html"><kbd>SLURM</kbd> Basics</a></center> |

<br />

#### References

(1) [Shebang (Unix)](https://en.wikipedia.org/wiki/Shebang_(Unix))  
(2) [Automate the Boring Stuff with Python](https://automatetheboringstuff.com/)
