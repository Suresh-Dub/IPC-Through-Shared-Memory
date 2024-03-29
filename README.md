<h1 align="center">IPC THROUGH SHARED MEMORY</h1>
Inter-process communication is the way by which multiple processes can communicate with each other. Shared memory in OS, message queues, FIFO, etc. are some of the ways to achieve IPC in os. A system can have two types of processes i.e. independent or cooperating. Cooperating processes affect each other and may share data and information among themselves.



## Flowchart

![App Screenshot](https://github.com/Suresh-Dub/OPERATING-SYSTEM-PROJECT/blob/main/Images/Shared%20Memory.png)


## Why Choose IPC through Shared Memory?

   ```bash
Usually, inter-related process communication is performed using Pipes or Named Pipes. And unrelated processes communication
can be performed using Named Pipes or through popular IPC techniques of Shared Memory and Message Queues.

But the problem with pipes, FIFO, and message queue is that the information exchange between two processes goes through the 
kernel, and it works as follows.

> The server reads from the input file.
> The server writes this data in a message using pipe, FIFO, or message queue.
> The client reads the data from the IPC channel, again requiring the data to be copied from the kernel's IPC buffer 
  to the client's buffer.
> Finally, the data is copied from the client's buffer.
> A total of four copies of data are required (2 read and 2 write). So, shared memory provides a way by letting two 
  or more processes share a memory segment. With Shared Memory, the data is only copied twice, from the input file 
  into shared memory and from shared memory to the output file.
   ```

## Algorithm of sharedmem2.c (Server Side Program) 

   ```bash
STEP – 1  In a C program first of all include all header files such as unistd.h , stdlib.h ,  stdio.h , string.h 
 Now include header file sys/shm.h to access shared memory system calls 

   ```
 ```bash
STEP – 2 Struct shared_use_st --- creating a shared_use_st of type struct  int data_available --- variable which indicates data is available  char message[Text_SZ] --- char array to hold the input string  main(){ 
 process_running = TRUE 
 void *shared_memory = (void *)0 

   ```
 ```bash
STEP – 3 shared_use_st *shared stuff --- structure which acts like a shared memory  char buffer[BUFSIZ] 
 int shmid --- varaiable for allocation of shared memory 
 shmid = shmget((key_t)1234, sizeof(struct shared_use_st) ,0666 | IPC_CREAt) ---  Allocation of Shared Memory 
 on failure shmget () returns -1

   ```
 ```bash
STEP - 4 Attachment of segement using shmat() system call 
 Shared_memory = shmat(shmid,(void *)0, 0) 
 On failure shmat() returns -1
 Shared_stuff = (struct shared_use_st *)shared_memory --- on successful  attachment of segment 
 Performing write operation 
 While(process_running) 
 While(shared_stuff->data_available == TRUE) 
 Sleep(1) 

   ```
 ```bash
STEP – 5 Server creates the string by asking the user an input 

   ```
 ```bash
STEP – 6 Reads the input and stores it in buffer 

   ```
 ```bash
STEP – 7 Writes it into the char array message[] 
 Check if data is available 

   ```
 ```bash
STEP – 8 Terminate the loop by using string “end” 
 Process_running = FALSE --- indicates that the data is written and sets the flag

   ```
 ```bash
STEP – 9 Now Detach segment using shmdt() system call 
 Exit(EXIT_SUCCESS) 

   ```
   
  
  ```bash

 } ---end of the server program 

   ```

 
 ## Algorithm of sharedmem1.c (Client Side Program) 
 
 ```bash
STEP – 1 In a C program first of all include all header files such as unistd.h , stdlib.h ,  stdio.h , string.h 
Now include header file sys/shm.h to access shared memory system calls. Now include header file sys/shm.h to access shared memory system calls. 
```
   
```bash
STEP – 2 Struct shared_use_st --- creating a shared_use_st of type struct  int data_available --- variable which indicates data is available  char message[Text_SZ] --- char array to hold the input string  main(){ 
 process_running = TRUE 
 void *shared_memory = (void *)0 

   ```
   
```bash
STEP – 3 shared_use_st *shared stuff --- structure which acts like a shared memory  int shmid --- varaiable for allocation of shared memory
   ``` 
   
 ```bash
 STEP – 4 shmid = shmget((key_t)1234, sizeof(struct shared_use_st) ,0666 | IPC_CREAt) ---  Allocation of Shared Memory 
 on failure shmget () returns -1 

   ``` 
   
   ```bash
STEP – 5 Attachment of segement using shmat() system call 
 Shared_memory = shmat(shmid,(void *)0, 0) 
 On failure shmat() returns -1 

   ``` 
   
   ```bash
STEP – 6 If shared_memory == (void *)-1 
 The exit(EXIT_FAILURE) 
 Shared_stuff = (struct shared_use_st *) shared_memory 
 Shared_stuff->data_available = FALSE 
 While process_running 
 If data available --- means data is available to read 

   ```
   
   
 ```bash
STEP – 7 Print data 
 Sleep(rand() %4) 
 Shared_stuff->data_available = FALSE --- clears the flag to show it has read the  data 

   ``` 
   
   ```bash
STEP – 8 Now Terminate the loop with the help of string “end” 

   ``` 
   
   ```bash
STEP – 9 At the end detach the shared memory with the help of shmdt() system call or  function 
 On failure shmdt() returns -1 
 Now Deallocating the shared memory with the help of shmctl() system call  Which on failure returns -1 
 Exit(EXIT_SUCCESS) 

   ```
   
 ```bash
 
 } --- End of Client Process

   ```


## Goal
To allow cooperating processes to access the same pieces of data concurrently and divides long tasks into smaller sub-tasks and can be executed in parallel.



## Writer Process
   ```bash
//Shared Memory for Writer Process
//Program 1: This program creates a shared memory segment, attaches itself to it and then writes some content into the shared memory segment.
//how it works?
/*shmget() function creates a segment with key 2345, size 1024 bytes and read and write permissions for all users. It returns the identifier of the segment which gets store in shmid. This identifier is used in shmat() to attach the shared segment to the address space of the process. NULL in shmat() means that the OS will itself attach the shared segment at a suitable address of this process.
Then some data is read from the user using read() system call and it is finally written to the shared segment using strcpy() function.*/
#include<stdio.h>
#include<stdlib.h>
#include<unistd.h>
#include<sys/shm.h>
#include<string.h>
int main()
{
int i;
void *shared_memory;
char buff[100];
int shmid;
shmid=shmget((key_t)2345, 1024, 0666|IPC_CREAT); //creates shared memory segment with key 2345, having size 1024 bytes. IPC_CREAT is used to create the shared segment if it does not exist. 0666 are the permisions on the shared segment
printf("Key of shared memory is %d\n",shmid);
shared_memory=shmat(shmid,NULL,0); //process attached to shared memory segment
printf("Process attached at %p\n",shared_memory); //this prints the address where the segment is attached with this process
printf("Enter some data to write to shared memory\n");
read(0,buff,100); //get some input from user
strcpy(shared_memory,buff); //data written to shared memory
printf("You wrote : %s\n",(char *)shared_memory);
}
   ```
## Reader Process
 ```bash
//Shared memory for Reader Process
//Program 2: This program attaches itself to the shared memory segment created in Program 1. Finally, it reads the content of the shared memory
//How it works?
/*shmget() here generates the identifier of the same segment as created in Program 1. Remember to give the same key value. The only change is, do not write IPC_CREAT as the shared memory segment is already created. Next, shmat() attaches the shared segment to the current process.
After that, the data is printed from the shared segment. In the output, you will see that it is the same data that you have written while executing the Program 1.*/
#include<stdio.h>
#include<stdlib.h>
#include<unistd.h>
#include<sys/shm.h>
#include<string.h>
int main()
{
int i;
void *shared_memory;
char buff[100];
int shmid;
shmid=shmget((key_t)2345, 1024, 0666);
printf("Key of shared memory is %d\n",shmid);
shared_memory=shmat(shmid,NULL,0); //process attached to shared memory segment
printf("Process attached at %p\n",shared_memory);
printf("Data read from shared memory is : %s\n",(char *)shared_memory);
}
   ```





##


## References 
<br> <a href="https://www.geeksforgeeks.org/inter-process-communication-ipc/">Geeks for Geeks</a> </br>
<br> <a href="https://www.tutorialspoint.com/what-is-interprocess-communication">Tutorial's Point</a> </br>




