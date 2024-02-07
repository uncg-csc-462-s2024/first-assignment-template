# Assignment 1: Linux Virtual Machine Installation and Simple Shell

***Note:*** *while I will assume you will install Ubuntu on top of VirtualBox, feel free to use other Linux distributions or virtual machine managers. The goal is to demonstrate that you are able to install a virtual machine and build/compile/run C code. All programming assignments will depend on this setup.*


***Note 2:*** *As part of your submission, you will edit this README.md file. All occurrences of [text like this] needs to be replaced by your edits. find markdown reference [here](https://www.w3schools.io/file/github-readme-image/).*


***Note 3:*** *even if you are running an Ubuntu system on desktop/laptop, you should install VirtualBox and then install Ubuntu on VirtualBox. The codes that you will be asked to write and run may interfere or ruin other programs, so some virtualization and isolation is necessary.*

## Submitter Information

Name: *[enter your full name]*

UNCG login ID: *[enter your UNCG login ID]*

Honor Pledge: *[enter your honor pledge. Find sample honor pledge text [here](https://uncg.instructure.com/courses/122861/pages/sample-honor-pledge).]*

## Part 1: Virtual Machine Installation


1. Install VirtualBox on your system. VirtualBox can be downloaded here <https://www.virtualbox.org/wiki/Downloads>
2. Download Ubuntu Desktop iso and save it somewhere on your host machine <http://www.ubuntu.com/download/desktop>. I recommend you download the latest LTS version, which is more stable than the non-LTS version.
3. Follow this [tutorial](https://ubuntu.com/tutorials/how-to-run-ubuntu-desktop-on-a-virtual-machine-using-virtualbox#1-overview) to install Ubuntu into VirtualBox (note that there will be some differences based on the version used in the tutorial and the version you install. **Use common sense!**)
4. Right-click the Ubuntu icon inside VirtualBox and go to Settings/Shared_Folder. Add a shared folder between your host machine and Ubuntu inside VirtualBox. Make sure that both systems can read/write on the folder. This will become handy in all of your programming assignments.  **Below, add snapshots of files shared between the two systems, for example, two windows showing the same contents.**

### Deliverable #1: Snapshots of files shared between host and virtual machine
*[add your images here]*

## Part 2: Simple Shell

In this assignment, you will write a *shell* for executing commands. A shell, as you should know, is a program that allows users to interact with operating system commands/other programs on the system.

This assignment is the first part of a two-part mini-project. __This part one consists of writing the shell input loop, parsing commands, and__ `fork/exec`. This, in most contexts, is known as a REPL (read-evaluate-print-loop). In __part two, we will extend this to include changing directories, piping, and redirection__, so it would be in your best interest to write your shell with some flexibility/adjustability!

### Here are the tasks required for the assignment.
1. In your `main` function, write code that infinitely loops until the user enters "quit". You should use either `fgets` (statically-allocated) or  `getline`. You can safely assume the user will not enter more than `LINE_MAX` (2048) characters. This is the read component of REPL. Inside the loop (after you have received input), pass the input string/buffer to a function `parse_cmd`.
2. Write the function `parse_cmd`. Its parameter, `input`, is the entered string from the read loop. We will assume that all commands are separated by a space, and thus, you will tokenize based on this delimiter. Create an array of (dynamically-allocated) strings, where each index is a tokenized string. Index 0 of this array should be the command, and indices 1...n-1 are the arguments to said command. For instance, if I enter `ls -l -a -t` , the resulting array of tokens is `array[0] = "ls"`, `array[1] = "-l"`, `array[2] = "-a"`, `array[3] = "-t"`. The actual implementation of this is up to you, but it's can be tricky to get it right. Below are some things to consider:

   
   1. You have to remember that you don't know how many arguments the command has, so you can't simply create a static array of strings.
   2. The issue, though, is that you also don't know how many to dynamically allocate.
   3. As a result, you should have some way of keeping track of the capacity of the arguments buffer, as well as the number of arguments being parsed.
   4. Then, as you're tokenizing, if you go over this capacity, you can simply double the size of the dynamically-allocated array (`man realloc` is very helpful!).
   5. Another very important hint is that you can't simply do something like `array[i] = buffer[i]` when copying over arguments from the input buffer because this isn't how you copy strings in C! There's a dedicated function this (`man strcpy`).
   6. Finally, pass this array of strings to a function `execute_cmd`. We will return to this function next, so don't forget about it!
3. Write the function `execute_cmd`. This should be a very short function (only around 8-10 lines including braces) consisting of a call to `fork` and a call to `exec`. You can use any variant of `exec`, but with the way we have set up things, your best bet is to use `execvp`. Note that this requires the last index of your array of strings to be `NULL` (i.e., using our `ls` example, this means `array[4] = NULL`), so you may need to slightly alter your `parse_cmd` function accordingly. The `exec` call should result in a child process that executes the given command and arguments. This is the evaluate and print components of REPL.
4. Lastly, in `parse_cmd`, free the dynamically-allocated array of strings. Since this is an array of strings, each element of this array is *also* dynamically allocated, meaning you can't simply call `free` on the array - you have to `free` each element first.

#### Other Helpful Information:
1. Keep your code structured, organized, and well-documented. This will help you later on (in life and in this class)!
2. Parsing and tokenizing the commands is the hardest part of this assignment. Check out this [video](https://uncg.hosted.panopto.com/Panopto/Pages/Viewer.aspx?id=2e496193-f2cf-4b4a-a70c-ae32001ff4c0&start=0) of a recitation section offered by Joshua Crotts, a TA for this course in a previous semester, and the respective [repository](https://github.com/joshuacrotts/c-session-demos-uncg-s22/tree/main/session03), for an example of tokenization, or simply use `man strtok`. It's not as intuitive as you may think.
3. Free all dynamically-allocated memory, and check for edge cases! This is why step 2 is the most complicated piece to get exactly right. Your program should not produce any warnings, memory leaks, or errors. `valgrind` and `gdb` are great tools for debugging.
4. Again, as stated in the prompt, you do not need to support changing directories, pipes, and redirection for this assignment.

### Deliverable #2: The code of your solution
All your code must be in `main.c'` and successful compile via `gcc -g -Wall -Werror main.c -o main.o`

## Part 3 (optional): Getting Familiar with Linux

If you are not familiar with Linux, this is the time to play around with Ubuntu. Find an intermediate-level Ubuntu tutorial online and follow it to learn more. You want to know these basic commands among others: `cd`, `mkdir`, `cc`/`gcc`, `cp`, `mv`, `rm`, `make`, `apt-get`, `sudo`.


