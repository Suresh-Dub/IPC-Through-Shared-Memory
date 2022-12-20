<h1 align="center">IPC USING SHARED MEMORY</h1>
Inter-process communication is the way by which multiple processes can communicate with each other. Shared memory in OS, message queues, FIFO, etc. are some of the ways to achieve IPC in os. A system can have two types of processes i.e. independent or cooperating. Cooperating processes affect each other and may share data and information among themselves.



## Flowchart

![App Screenshot](https://github.com/Suresh-Dub/OPERATING-SYSTEM-PROJECT/blob/main/Images/Shared%20Memory.png)




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

## Team Members
<br> <a href="https://github.com/Akshay932004">Akshay Pawar</a> </br>
<br> <a href="https://github.com/manujkhajuria">Manuj Khajuria</a> </br>
<br> <a href="https://github.com/suresh-dub">Suresh Dub</a> </br>


