Download Link: https://assignmentchef.com/product/solved-cs332-lab2-process-management-system-calls
<br>
<strong>Process Management System Calls  </strong>

<ul>

 <li>A process is basically a single running program.</li>

 <li>Each running process has a unique number – a process identifier, ​<strong>pid</strong> ​ (​ an integer value).</li>

 <li>Each process has its own address space.</li>

 <li>Processes are organized hierarchically. Each process has a ​<strong>parent </strong>​process that explicitly arranged to create it. The processes created by a given parent are called its ​<strong>child </strong>​processes</li>

 <li>The C function ​<strong>getpid</strong>()​ will return the ​ ​<strong>pid</strong> ​ of the process.​</li>

 <li>A child inherits many of its attributes from the parent process.</li>

 <li>The UNIX command ​ps will list all current processes running on your machine and will list the ​ ​</li>

 <li>​<strong>Remark: </strong>​When UNIX is first started, there is only one visible process in the system. This process is called ​<strong>init</strong> with​ ​pid 1.​ The only way to create a new process in UNIX is to duplicate an existing process, so ​init is the ancestor of all subsequent processes.​</li>

</ul>

<h1>Process Creation</h1>

<ul>

 <li>Each process is named by a process ID number</li>

 <li>A unique process ID is allocated to each process when it is created</li>

 <li>Processes are created with the ​<strong>fork()</strong> ​ system call (the operation of creating a new process is some-​ times called forking a process)</li>

 <li>The ​fork() system call does not take an argument​</li>

 <li>If the ​fork() system call fails, it will return -1​</li>

 <li>If the ​fork() system call is successful, the process ID of the child process is returned in the parent​ process and a 0 is returned in the child process</li>

 <li>When a ​fork() system call is made, the operating system generates a copy of the parent process​ which becomes the child process</li>

 <li>Some of the specific attributes of the child process that differ from the parent process are:

  <ul>

   <li>The child process has its own unique process ID</li>

   <li>The parent process ID of the child process is the process ID of its parent process</li>

   <li>The child process gets its own copies of the parent process’s open file descriptors. Subsequently changing attributes of the file descriptors in the parent process won’t affect the file descriptors in the child, and vice versa</li>

  </ul></li>

 <li>When the lifetime of a process ends, its termination is reported to its parent and all the resources including the PID is freed.</li>

</ul>

<h1>Example of fork() ​ system call structure​</h1>

Here a process P calls <sup>fork()</sup>​ ,​ the operating system takes all the data associated with P, makes a brand- new copy of it in memory, and enters a new process Q into the list of current processes. Now both P and Q are on the list of processes, both about to return from the <sup>fork() </sup>​ call, and they continue.​

The <sup>fork() </sup>​ system call returns Q s process ID to P and 0 to Q. This gives the two processes a way of​      doing different things. Generally, the code for a process looks something like the following: int child = fork();  if(child == 0) {

//code specifying how the child process Q is to behave

}else {

//code specifying how the parent process P is to behave

}

<h1>Process Identification</h1>

<ul>

 <li>You can get the process ID of a process by calling getpid()​</li>

 <li>The function getppid() ​ returns the process ID of the parent of the current process​</li>

 <li>Your program should include the header files h ​ and ​ sys/types.h ​       to use these functions​</li>

</ul>

<h1>waitpid() System Call​</h1>

<ul>

 <li>A parent process usually needs to synchronize its actions by waiting until the child process has either stopped or terminated its actions</li>

 <li>The waitpid() system call gives a process a way to wait for a process to stop. It’s called as follows.</li>

</ul>

pid = waitpid(child, &amp;status, options);

In this case, the operating system will block the calling process until the process with ID child ends

<ul>

 <li>The options ​ parameter gives ways of modifying the behavior to, for example, not block the calling​ We can just use 0 for options ​         ​here.</li>

 <li>When the child​ process ends,​ the operating system changes the int variable status​        to​ represent how child process ended (incorporating the exit code, should the calling process want that information), and it unblocks the calling process, returning the process ID of the process that just stopped.</li>

</ul>




<ul>

 <li>If the calling process does not have any child associated with it, <strong>wait</strong>​ will return immediately with a​ value of -1</li>

</ul>

<ol>

 <li>Write a program ​ <strong>processes.c​</strong>​ , and let the parent process produce two child processes.</li>

</ol>

One prints out “I am first child, my pid is: ” PID, and the other prints out “I am second child, my pid is: ” PID.

Guarantee that the parent terminates after the children terminate (Note, you need to wait for two child processes here). Use the getpid() function to retrieve the PID.

<strong>2 </strong>Consider​ the parent process as P. The program consists of fork() system call statements placed at different points in the code to create new processes Q and R. The program also shows three variables: a, b, and pid – with the print out of these variables occurring from various processes. Show the values of pid, a, and b printed by the processes P, Q, and R.

int a=​ 10​ , b=​ 25​ , fq=​ 0​ , fr=​               0​

fq=fork() // fork a child – call it Process Q​                 if(​ fq==0​ ){​ // Child successfully forked​             a=a+b print values of a, b, and​ process_id​                 fr=fork() // fork another child – call it Process R​           if​ (​ fr!=0​ ){​     b=b+15​

//print values of a, b, and process_id​

}else​             {​     a=(a*b)+20​

//print values of a, b, and process_id​

}

}else​  {​   b=a+b-5​ ;​    print values of a, b, and​ process_id​        }