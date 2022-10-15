# OPERATING-SYSTEM-PROJECT
                                              IMPLEMENTATION OF IPC USING SHARED MEMORY
Inter-process communication in OS is the way by which multiple processes can communicate with each other. Shared memory in OS, message queues, FIFO, etc. are some of the ways to achieve IPC in os. A system can have two types of processes i.e. independent or cooperating. Cooperating processes affect each other and may share data and information among themselves.  
Interprocess Communication or IPC provides a mechanism to exchange data and information across multiple processes, which might be on single or multiple computers connected by a network.If a longer job has been assigned to the CPU then many shorter jobs after it will have to wait. This algorithm is used in most batch operating systems.

CHARACTERISTICS : Characteristics Of Inter-process Communication:
There are mainly five characteristics of inter-process communication in a distributed environment/system.

Synchronous System Calls:
In the synchronous system calls both sender and receiver use blocking system calls to transmit the data which means the sender will wait until the acknowledgment is received from the receiver and receiver waits until the message arrives.
Asynchronous System Calls:
In the asynchronous system calls, both sender and receiver use non-blocking system calls to transmit the data which means the sender doesn’t wait from the receiver acknowledgment.
Message Destination:
A local port is a message destination within a computer, specified as an integer. Aport has exactly one receiver but many senders. Processes may use multiple ports from which to receive messages. Any process that knows the number of a port can send the message to it.
Reliability:
It is defined as validity and integrity.
Integrity:
Messages must arrive without corruption and duplication to the destination.
Validity:
Point to point message services are defined as reliable, If the messages are guaranteed to be delivered without being lost is called validity.
Ordering:
It is the process of delivering messages to the receiver in a particular order. Some applications require messages to be delivered in the sender order i.e the order in which they were transmitted by the sender.
  
  SYSTEM CALLS USED : 
  ftok(): is use to generate a unique key.

shmget(): int shmget(key_t,size_tsize,intshmflg); upon successful completion, shmget() returns an identifier for the shared memory segment.

shmat(): Before you can use a shared memory segment, you have to attach yourself
to it using shmat(). void *shmat(int shmid ,void *shmaddr ,int shmflg);
shmid is shared memory id. shmaddr specifies specific address to use but we should set
it to zero and OS will automatically choose the address.

shmdt(): When you’re done with the shared memory segment, your program should
detach itself from it using shmdt(). int shmdt(void *shmaddr);

shmctl(): when you detach from shared memory,it is not destroyed. So, to destroy
shmctl() is used. shmctl(int shmid,IPC_RMID,NULL);
  
    IMPLEMENTATION : 
We know that to communicate between two or more processes, we use shared memory but before using the shared memory what needs to be donewith the system calls, let us see this −

* Create the shared memory segment or use an already created shared memory segment (shmget())

* Attach the process to the already created shared memory segment (shmat())

* Detach the process from the already attached shared memory segment (shmdt())

* Control operations on the shared memory segment (shmctl())


