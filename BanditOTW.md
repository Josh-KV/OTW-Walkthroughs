Bandit Level 0
---
> The goal of this level is for you to log into the game using SSH. The host to which you need to connect is bandit.labs.overthewire.org, on port 2220. The username is bandit0 and the password is bandit0. Once logged in, go to the Level 1 page to find out how to beat Level 1.

*SSH, or Secure Shell, allows a client to establish an encrypted communication link to a remote server*

To use SSH, we open the terminal and and type  `ssh username@serverName.ext` where `username` is the user we want to log in to on the remote server, and `serverName.ext` is the remote server that we intend to connect to. Lastly, we can use the `-p` flag followed by a number to specify a port. If this flag is not used the port is set to the default for SSH, port 22.

In this case, we want to log in to user `bandit0` on `bandit.labs.overthewire.org`, using port `2220`.

Thus, the command is `ssh bandit0@bandit.labs.overthewire.org -p 2220`.

If this is the first time connecting to the remote server, you will be prompted to accept the fingerprint of the remote server. You will then be prompted for the password for `bandit0`, which is `bandit0`. Note that characters may not be displayed while typing in the password.

Bandit Level 0 &rarr; 1
---
> The password for the next level is stored in a file called **readme** located in the home directory. Use this password to log into bandit1 using SSH. Whenever you find a password for a level, use SSH (on port 2220) to log into that level and continue the game.

When we log in to `bandit0`, we are in the home directory. The `~` symbol in `bandit0@bandit:~$` indicates that our current directory is the home directory.

To list the contents of the current directory we can type `ls` which is short for "list".

To view the contents of the file **readme**, we can type `cat readme` to print the file contents. Usage and documentation for commands can be found in the "man" pages or manual pages for a given command by typing `man someCommand`.

Once we see the password, copy it and exit the remote server by typing `exit`. This should prompt `logout` and then confirm that your connection to bandit.labs.overthewire.org has been closed.

Bandit Level 1 &rarr; 2
---
> The password for the next level is stored in a file called **-** located in the home directory.

First we must log in to user `bandit1` using the password.
<details><summary>Bandit1 Password</summary>NH2SXQwcBdpmTEzi3bvBHMM9H66vVXjL</details>

Next we notice that the file name is a dash, which is typically used to specify further arguments for a command, like how we used `-p` to specify the port number.

To clarify that we want `cat` to concatenate and output the contents of the file named "-", we can use the full name of the file, which includes its location. We can view our current directory by typing `pwd` which yields `/home/bandit1/`. Then we can type `cat /home/bandit1/-` to reveal the password.

A shortcut for typing the current working directory is `.`, and so we can equivalently type `cat ./-`.

Bandit Level 2 &rarr; 3
---
> The password for the next level is stored in a file called **spaces in this filename** located in the home directory.
<details><summary>Bandit2 Password</summary>rRGizSaX8Mk1RTb1CNQoXTcYZWU6lgzi</details>

We can avoid misinterpretation of spaces by writing the file name in quotes like `cat "spaces in this filename"`, or we can use backslashes to "escape" the space characters which works just as well and can be autocompleted pressing TAB on the keyboard after typing the first few letters. The result is `cat spaces\ in\ this\ filename`.

Bandit Level 3 &rarr; 4
---
> The password for the next level is stored in a hidden file in the **inhere** directory.
<details><summary>Bandit3 Password</summary>aBZ0W5EmUfAf7kHTQeOwd8bauFJ2lAiG</details>

We begin in the home directory for user `bandit3` and use the `ls` command to list all files and directories. This reveals the **inhere** directory.

To change our working directory to "**inhere**", we use the `cd`, or "change directory", command followed by the directory we want to change to. In this case we type `cd inhere`.

If we use `ls` to list the directory contents, the directory appears to be empty. However, the clue indicated that there is a hidden file in this directory. To list hidden files and directories we use the `-a` flag for the `ls` command which lists all files and directories *including* hidden ones. So altogether we type `ls -a` which not only reveals a `.hidden` file, but two directories denoted `.` and `..`. These hidden "dot" directories provide a way to navigate or reference the current directory, `.`, or the parent directory `..`.

Lastly we can print the contents of the hidden file with `cat .hidden` which reveals the password.

Bandit Level 4 &rarr; 5
---
> The password for the next level is stored in the only human-readable file in the **inhere** directory. Tip: if your terminal is messed up, try the “reset” command.
<details><summary>Bandit4 Password</summary>2EW7BBsr6aMMoJ2HjW067dm8EgX26xNe</details>

We begin again by changing directory to "**inhere**". Next, we list the contents and see that there are many files inside. Rather than checking each file for the password, we can use the hint that the file with our password is the only human-readable file. The `file` command describes the contents of a single file or multiple files. To use the `file` command on all of the files in a directory we can type `file *` where `*` represents the entire directory contents. 

However, the files in this directory are named in a way that interferes with the `file` command. When we run the command `file *`, what actually runs is `file -file00`, `file -file01`, `file -file02`, and so on. This is a problem because the dash character is read as an indication of a flag, and so the `file` command thinks we want to run `file -f ile00` where `-f` is a flag rather than part of our file name.

In order to avoid this problem we can use the path name for the files which is  `./"filename"` for files in the current working directory. Instead of choosing a file name, we use `*` to run the command on all files. Thus the working command to describe the contents of each file in this directory is `file ./*`.

Now we can see that only one file is described as `ASCII text`. This file contains the password which we reveal with `cat ./-file07`.

Since human-readable files are often encoded as ASCII (American Standard Code for Information Interchange), we could have also searched only for output lines from the `file` command that included "ASCI" which we can do by adding `| grep "ASCII"` onto our file command to get `file ./* | grep "ASCII"` which performs the file command and then "pipes" the result using `|` to `grep` which only returns the output with lines including the text in quotes.

Bandit Level 5 &rarr; 6
---
> The password for the next level is stored in a file somewhere under the **inhere** directory and has all of the following properties: human-readable, 1033 bytes in size, and not executable.
<details><summary>Bandit5 Password</summary>lrIWWI6bB37kxfiCQZqUdOIYfr6eEeqR</details>

For this level, we learn a few of the flags that can be used with the `file` command. First, we can either change directory to the `inhere` directory and perform the `file` command there, or we can perform the file command from the current home directory and specify the location as the first argument of the `file` command. The latter looks like this: `file \inhere`.

Since we are told that the password file must be human-readable, 1033 bytes in size, and not executable, we can use the following flags
- `-readable`
- `-size`
- `-executable`

The `-readable` flag returns only human-readable files. The `-size someSize` flag allows you to specify a file size. In this case 1033 bytes, using "c" to denote bytes. Other size notations are listed in the man page for reference (`man file`). Lastly, the `-executable` flag can be negated with `\! -executable` to only return files that are *not* executable.

Thus, altogether we have `find \inhere -readable -size 1033c \! -executable`. This reveals the password file, which we can display the contents of using the `cat` command. Note that since we have not changed our working directory to the directory containing the file, we must either do so, or specify the file location. To do so in this case we type: `cat inhere/maybehere07/.file2`.

Bandit Level 6 &rarr; 7
---
> The password for the next level is stored **somewhere on the server** and has all of the following properties: owned by user bandit7, owned by group bandit6, 33 bytes in size.
<details><summary>Bandit6 Password</summary>P4L4vucdmLnm8I7Vl7jG1ApGSfjYKqJU</details>

Next, we want to use the `find` command again. This time the location we specify will be the root directory, which is the highest level directory on the server. This is denoted by `/` and contains all directories and files in the server's file-system.

Next, we must use the `-size`, `-user`, and `-group` flags to specify the properties of our desired password file.

At this point we may run the command `find / -size 33c -user bandit7 -group bandit6`. Sorting through some error messages we can find a file called `bandit7.password` which contains the password.

These error messages let us know which locations the find command run by our user did not have permission to search. However, for our purposes these error messages make it difficult to find the desired file. Thus, as part of the command, we can take the error output which comes from stdErr denoted "2" and send it to our devices null directory with `2>/dev/null`. Altogether we have `find / -size 33c -user bandit7 -group bandit6 2>/dev/null` which reveals the password file: `/var/lib/dpkg/info/bandit7.password`.  We can then output the file as in the last level by telling the `cat` command the file and its location since our current working directory is not the same as the file's. This looks like: `cat /var/lib/dpkg/info/bandit7.password`

Bandit Level 7 &rarr; 8
---
> The password for the next level is stored in the file **data.txt** next to the word **millionth**.
<details><summary>Bandit7 Password</summary>z7WtoNQU2XfjmMtWA8u5rN4vzqu4v99S</details>

We begin by listing the contents of the home directory with `ls` and see the `data.txt` file. We could try to view the file contents with the `less data.txt` command which shows us the contents in a scrollable view which can be exited with the '**q**' key.

However, the file is much too large for manual searching. We must use the `grep` command to find the word "millionth" which we are told will be in the same line as the password. The grep tool can take a pattern argument in quotes as well as a file. Thus, we type `grep "millionth data.txt"`. This command will show us only the line(s) containing the text "millionth", which in this case reveals the password for user `bandit8` as desired.

Bandit Level 8 &rarr; 9
---
> The password for the next level is stored in the file **data.txt** and is the only line of text that occurs only once.
<details><summary>Bandit8 Password</summary>TESKZC0XvTetK0S9xNwm25STk5iWrBvP</details>

As in the previous level, we are working with a data.txt file in the home directory. Manually searching for the only unique line of the file is unreasonable, so we must use a few more text handling tools.

The `sort` command takes lines of text from a file and sorts them alphabetically by default. We can use this to group together lines that occur multiple times.

Next we can use the `uniq` command. By default, the `uniq` command merges matching ***adjacent*** lines to the first occurrence of the matching line. We can use the `-u` flag to change this behavior to only print unique lines. This will reveal the password since it is the only line of text that occurs only once.

To use both tools we begin with `sort data.txt` to group the repeated lines together and pipe the output to the `uniq` tool with the `-u` flag. Altogether we have: `sort data.txt | uniq -u` which reveals the bandit9 password as desired.

*Note that we cannot use only the `uniq -u` command on the `data.txt` file since the `uniq` tool only compares adjacent lines.*

Bandit Level 9 &rarr; 10
---
> The password for the next level is stored in the file **data.txt** in one of the few human-readable strings, preceded by several ‘=’ characters.
<details><summary>Bandit9 Password</summary>EN632PlfYiZbn3PhVK3XOGSlNInNE00t</details>

For this level we can use the `strings data.txt` command to reveal all human-readable strings in the file.

To further simplify our search, we can use grep to output only lines containing several "=" characters, say at least two: `grep "=="`.

Altogether we have `strings data.txt | grep "=="` which reveals a few lines, the last of which is clearly the password.

Bandit Level 10 &rarr; 11
---
> The password for the next level is stored in the file **data.txt**, which contains base64 encoded data.
<details><summary>Bandit10 Password</summary>G7w8LIi6J3kTb8A7j9LgrywtEUlyyp6s</details>

For this level we must decode the base64 encoded file, **data.txt**. Read more about base64 encoding schemes [here](https://en.wikipedia.org/wiki/Base64#Variants_summary_table).

We can use the `base64` command with the `-d` flag to decode the file as follows: `base64 -d data.txt` which reveals the password for user bandit10 as desired.

Bandit Level 11 &rarr; 12
---
> The password for the next level is stored in the file **data.txt**, where all lowercase (a-z) and uppercase (A-Z) letters have been rotated by 13 positions.
<details><summary>Bandit11 Password</summary>6zPeziLdR2RKNdNYFNb6nVCKzphlXHBM</details>

The encoding "Rot13" described above can be achieved using `tr`, the translate characters command. The `tr` command accepts two sets of characters from an input stream and outputs the stream with all occurrences of characters in the first set, to the corresponding characters from the second set.

The `tr` command accepts sets of consecutive characters like `'abcd'` in abbreviated  form: `'a-d'`. The `tr` command also accepts multiple abbreviated, appending consecutive sets so that `A-Ca-c` is equivalent to `ABCabc`.

Thus, we can decode a Rot13 cipher by translating all characters of the encoded cipher, both upper and lower case, back by 13 characters. That is `'N-ZA-Mn-za-m'` should be mapped to `'A-Za-z'`.

We now use the `tr` command on our **data.txt** file as follows: `cat data.txt | tr 'N-ZA-Mn-za-m' 'A-Za-z'`. This reveals the password and the level is complete.

Bandit Level 12 &rarr; 13
---
> The password for the next level is stored in the file **data.txt**, which is a hexdump of a file that has been repeatedly compressed. For this level it may be useful to create a directory under /tmp in which you can work using mkdir. For example: mkdir /tmp/myname123. Then copy the datafile using cp, and rename it using mv (read the manpages!)
<details><summary>Bandit12 Password</summary>JVNBBFSmZwKKOP0XbFXOoW8chDz5yVRv</details>

A hexdump is simply binary data represented in hexadecimal format. More specifically, a hexdump displays bits of binary data in groups of two hexadecimal digits. For example, the bits `11010001` are shown as `D1` in hexadecimal.

Looking at **data.txt** with the `less data.txt` command, we see that this file is a typical hexdump which, in addition to displaying some data represented in hexadecimal in the center column, also displays addresses in the left column and a text representation in the right column.

We will now revert the hexdump back to a binary file and then uncompress until we reach an uncompressed file containing the password for `bandit13`.

First we must make a temp directory in which we may modify and create files. As instructed, we do this in the `/tmp` directory with the make directory command `mkdir`. We will call ours "plume" which we create with the command `mkdir /tmp/plume`. Next, with home still set as our working directory, we copy the datafile to the newly created directory: `cp data.txt /tmp/plume`. Now we can change directory with `cd /tmp/plume` to make working with the file easier since we don't have to specify a file's location for each command if it is in our working directory.

To reverse a hexdump we use the `xxd` command with the `-r` flag for "reverse". We can provide an input file and specify the output filename as follows: `xxd -r data.txt unDump`.

Next, we type `file unDump` to see that the resulting output is a binary file, formerly named data2.bin, which has been compressed with gzip.
