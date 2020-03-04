# Shell-Implementation

Command Execution
Implement basic command execution. You will want to look at the manual page for the exec(3) function by typing "man 3 exec" Once this is done, you should be able to use your shell to run single commands, such as:

  cs6233> ls
  cs6233> grep cat /usr/share/dict/words
Hint:
You will notice that there are many variants on exec(3). You should read through the differences between them, and then choose the one that allows you to run the commands above -- in particular, pay attention to whether the version of exec you're using requires you to enter in the full path to the program, or whether it will search the directories in the PATH environment variable.

I/O Redirection
Now extend the shell to handle input and output redirection. Programs will be expecting their input on standard input and will write to standard output, so you will have to open the file and then replace standard input or output with that file. As before, the parser already recognizes the '>' and '<' characters and builds a redircmd structure for you, so you just need to use the information in that redircmd to open a file and replace standard input or output with it.

Hints:
Look at the dup2(2) and open(2) calls.
The file descriptor the program is currently using for input or output is available in rcmd->fd .
If you're confused about where rcmd->fd is coming from, look at the redircmd function and remember that 0 is standard input, 1 is standard output.
Be careful with the open call; in particular, make sure you read about the case when you pass the O_CREAT flag.
When this is done, you should be able to redirect the input and output of commands:

  cs6233> ls > a.txt 
  cs6233> sort -r < a.txt
Pipes
The final task is to add the ability to pipe the output of one command into the input of another. You will fill out the code for the '|' case of the switch statement in runcmd to do this.

Hints:
The parser provides the left command in pcmd->left and the right command in pcmd- >right .
Look at the fork(2), pipe(2), close(2) and wait(2) calls.
If your program just hangs, it may help to know that reads to pipes with no data will block until all file descriptors referencing the pipe are closed.
Note that fork(2) creates an exact copy of the current process. The two process share any file descriptors that were open at the time the fork occurred.
Once this is done, you should be able to run a full pipeline:

  cs6233> cat /usr/share/dict/words | grep cat | sed s/cat/dog/ > doggerel.txt
  cs6233> grep con < doggerel.txt
