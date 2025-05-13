# client-server-experiments
This repository contains coursework from my Computer Systems class at Southwestern University. All code is original unless otherwise noted. This code was produced as a collaboration between Zoe Van Der Vlugt and Timothy Berlanga. Do not submit this code as your own in any academic setting.

## Summary
In this project, our goal was to write several client and server programs that furthered our knowledge of client/server interactions, threads & processes, and improve our pair programming skills. We created four
files:
* **echoServer.c:** responds to each client it connects to with an “echo” of the message the
client sends in both the server console and back to the client. When it receives the word kill, the
server terminates.

* **forkSessionServer.c:** forks a separate process in order to handle each client connection.
Each child process handles one client, while the parent waits/listens for new connections.
Sending a kill command terminates child processes, but the word killserver kills the original
parent process (without killing pre-existing children).

* **threadSessionServer.c:** causes new threads to be launched each time there is a new
client. It uses POSIX threads rather than forking new processes. The kill and killserver
commands function the same as in forkSessionServer.c.

* **sessionClient.c:** connects to a server, listens for two startup messages, and then takes
client input, sends it to the server, and outputs the server’s response until the client enters an
exit command. This program works identically with forkSessionServer.c and
threadSessionServer.c, and responds to the kill and killserver commands. The first message the
client receives will instruct these commands, and the second will be a unique ID that specifies
which process/thread it is communicating with.

## Additional Resources
* [1] https://www.geeksforgeeks.org/what-is-ports-in-networking/#
* [2] https://www.fortinet.com/resources/cyberglossary/what-is-ip-address
* [3] https://www.geeksforgeeks.org/socket-in-computer-network/
* [4] https://stackoverflow.com/questions/152457/what-is-the-difference
* [5] https://docs.oracle.com/cd/E19455-01/805-7228/6j6q7ueov/index.html
* [6] https://www.backblaze.com/blog/whats-the-diff-programs-processes-and-threads/
* [7] https://www.geeksforgeeks.org/difference-between-process-and-thread/#
* [8] https://www.geeksforgeeks.org/mutex-lock-for-linux-thread-synchronization/
* [9] https://www.cs.kent.edu/~ruttan/sysprog/lectures/multi-thread/multi-thread.html#:~:text=
* [10]https://stackoverflow.com/questions/61622290/process-child-creating-a-new-session
* [11]https://www.ibm.com/docs/en/zos/2.4.0?topic=functions-setsid-create-session-set-process-g
roup-id
* [12]https://stackoverflow.com/questions/1068849/how-do-i-determine-the-number-of-digits-of-an
-integer-in-c
* [13]https://aljensencprogramming.wordpress.com/2014/05/15/the-kill-function-in-c/
* [14]https://stackoverflow.com/questions/19201689/naming-threads-using-typedef-struct
* [15] https://cplusplus.com/reference/cstdio/snprintf/
