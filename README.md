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


## Specifics
* What are ports, IP addresses, and sockets, and what are the differences between
the three?
<br>
> A port is a logical address of a 16-bit unsigned integer that is assigned to every application on a computer that uses the internet to send or receive data [1]. Internet Protocol (IP) addresses are unique identifying numbers assigned to all devices connected to the internet[2]. A socket is one endpoint of a communication link between two programs on a network [3]. IPs allow data to be received from the internet to a device, ports allow the data to be delivered to the correct application on the device, and sockets are the endpoint defined by both an IP and
port [4].
* Describe what these defaultServer.c and defaultClient.c do. Discuss the similarities and differences between them.
<br>
> A server is a system that provides services to other systems in its network, and a client
is a system that uses remote services from a server [5]. Both have a buffer that is cleared after
printing messages and after connecting, and both have to read from and write to each other.
The server’s main function has a port parameter that allows it to open a socket; It ‘listens’ for a
client, and should accept and open a socket when one searches on the network, otherwise it will
print an error message on where the connection went wrong. Once it’s connected to a client, the
buffer is cleared and it will print “Here is the message:”
, while it sends to the client “I got your
message.
” The client attempts to open the socket and get the host, otherwise prints an error message with the issue. 
After a successful connection, it displays “Please enter the message: ,
”
and then the buffer is cleared. Whatever is inputted by the user is read to the buffer and then
sent to the server. Then the buffer is cleared, and whatever the server sends is read and printed
to the user.
client.
echoServer.c
* How different is this program from the defaultServer.c?
<br>
> The only difference is that there is an infinite while loop for communicating with the
* a. What aspects of the original program had to change, and which aspects did
you change to make the code more elegant and understandable?
<br>
> After connecting to the client, there is an infinite while loop that ‘listens’ for a message
and reads what is sent, then prints the message on its display. If the message is kill, it will print
“received ‘kill’ command. Terminating server.
” and exit the loop to terminate. Otherwise, it will
write back to the client “I got your message.
”
sessionClient.c
* How is this program different from defaultClient.c?
<br>
> After receiving the first message from the server, the client displays the startup message
that describes the kill and killserver commands. Then, it enters a loop for communicating–
Within the loop it receives the process/thread ID and displays it, then takes & sends input, and
receives and prints messages from the server.
* a. Why does this client program not function well with the defaultServer.c
or the echoServer.c?
<br>
> This client is based around receiving two initial messages: One for the kill commands,
and one for the process/thread ID. Those two servers don’t have those starting messages.
* b. Also, think of a useful server application (the ones in this project are just
simple examples) that you could design, and that this exact same client
program could still usefully interface with.
<br>
> One server application could be where the server controls access to a system of files,
and the first message that the client receives instructs what commands can access, read,
delete, and write on files, as well as the usual kill commands. After that, the process/thread ID
would continue to be displayed, and the client and server would communicate as the user sends
commands.
forkSessionServer.c
* How did you make the parent and child processes behave differently?
<br>
> There is a loop within the outer parent loop each time there is communication with the
client that is the child process. The parent process doesn’t do anything other than create new
child processes when a new client is connected to the parent.
* a. When "killserver" was sent to a child process, how did it stop the parent
process?
<br>
> It sends a signal to the parent process through its ID to terminate it using the kill
command. Also, it set a boolean variable to true which keep track of if the server had been
killed. After the server had been killed, the while loop accepting clients while no longer run
because it only runs while the server is not killed.
* Explain the difference between processes and threads.
<br>
> A process is a program loaded into memory with all of the resources it needs to operate
[6]. A thread is the unit of execution inside a process [6]. So, threads are smaller segments
within processes, they share memory & data, are quicker to create, and are more efficient for
communication [7].
threadSessionServer.c
* What was the general structure of this program? How did threads serving clients
tell the main server to stop accepting connections?
<br>
> This program was set up in a similar way to the fork server, except we used our thread
function as a function to handle the client’s connection of the thread. This had the while loop that
connected the client to the server until the client entered kill. We used a boolean to keep track of
when the killserver command was sent by the client, and that variable was the condition for the
while loop that accepted new client connections.
* a. Was any synchronization needed? Why/why not?
<br>
> We did not use any mutex locks to cause synchronization, although we could have used
them to ensure that threads do not not simultaneously execute a critical section [8]. It could
have helped because the main thread reads requests from clients and sends them to new
threads to be handled– These threads need to be notified when there is new inputted
information, otherwise they should be waiting [9]. We did utilize the sleep command to ensure
the thread ID could be communicated back to the client after they sent a message. We also
made use of the typedef struct to store the client thread id. This helped us ensure that each
client's unique thread id was preserved when new clients were created.[14]
* Compare the process and thread approaches to coding the server. What were the
pros and cons of each approach? Give your opinion about which approach was
easier/harder, and why.
<br>
> We thought that the threading approach was more difficult to code, maybe because we
couldn’t make use of the fork method that we had used in a previous project. Forking did not
require synchronization or the use of a client function. We were not as comfortable with threads,
but writing the code was not as difficult after implementing correct functionality into our fork
server. The threading method allows for quicker formation and communication, but doesn’t allow
for isolation the way that processes can. Creating new processes to handle clients can be
excessive overhead for some situations.
Other
* Discuss any new C functions you had to teach yourself how to use in order to
complete the project.
<br>
> strcmp() - This command is used to make a comparison between two strings. However, c
does not have string types as it instead makes use of string of chars.
setsid() - this function helps ensure that the child process can continue running even
after the parent terminates. It essentially detaches the child from the parent. This helped
fix issues in communication between the server and client(such as the weird issue after
two messages) and handling of kill/killserver. [10][11]
kill () - This command is used to kill the parent process. [13]
pthread
_
create - This function is used to create a thread. One of the arguments is a
defined thread creation function. We used this function to handle our client connections.
The typedef struct was useful to store the unique client thread[14]
pthread
self - This function retrieves the thread id.
_
* What questions do you still have?
<br>
> What are more complex implementations of a server? How would we go about setting up
a connection without buzz? How involved would it be to create a server-client connection that
responds with different text depending on the input?

* At the end of the report, provide information about any help you received. List
every website you get help from. Explain the SPECIFICS of the help you receive.
<br>
> * [1] https://www.geeksforgeeks.org/what-is-ports-in-networking/#
> * [2] https://www.fortinet.com/resources/cyberglossary/what-is-ip-address
> * [3] https://www.geeksforgeeks.org/socket-in-computer-network/
> * [4] https://stackoverflow.com/questions/152457/what-is-the-difference
> * [5] https://docs.oracle.com/cd/E19455-01/805-7228/6j6q7ueov/index.html
> * [6] https://www.backblaze.com/blog/whats-the-diff-programs-processes-and-threads/
> * [7] https://www.geeksforgeeks.org/difference-between-process-and-thread/#
> * [8] https://www.geeksforgeeks.org/mutex-lock-for-linux-thread-synchronization/
> * [9] https://www.cs.kent.edu/~ruttan/sysprog/lectures/multi-thread/multi-thread.html#:~:text=
> * [10]https://stackoverflow.com/questions/61622290/process-child-creating-a-new-session
> * [11]https://www.ibm.com/docs/en/zos/2.4.0?topic=functions-setsid-create-session-set-process-g
roup-id
> * [12]https://stackoverflow.com/questions/1068849/how-do-i-determine-the-number-of-digits-of-an
-integer-in-c
> * [13]https://aljensencprogramming.wordpress.com/2014/05/15/the-kill-function-in-c/
> * [14]https://stackoverflow.com/questions/19201689/naming-threads-using-typedef-struct
> * [15] https://cplusplus.com/reference/cstdio/snprintf/

<br>
I have acted with honesty and integrity in producing this work and am unaware of anyone who has not.