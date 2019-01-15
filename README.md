###  README --- Project3Skeleton/

In Phase 3 of your class, you will implement a Key-Value Store that runs on a single node on *Amazon EC2*. As in phases 1 and 2, you will submit an initial design document and a final design document apart from the code itself.

![Architecture Diagram][1] 
### Skeleton Code:

The project skeleton you **should** build on top of is posted at <https://github.com/CS162Berkeley/Project3Skeletion>. If you have git installed on your system, you can run <tt>git clone https://github.com/CS162Berkeley/Project3Skeletion.git</tt>, or else you can download the tarball from [here][2]. You can define additional classes and methods as you deem fit.

### EC2 instructions:

Read the instructions carefully from [this][3] document regarding instantiation, operation, and termination of EC2 instances. As always, let us know in Piazza if anything is unclear.

### Requirements:

*   Your KeyValue Server will support 3 interfaces: 
    *   **Value GET (Key k)**: Retrieves the key-value pair corresponding to the provided key.
    *   **PUT(Key k, Value v)**: Inserts the key-value pair into the store.
    *   **DEL(Key k)**: Removes the key-value pair corresponding to the provided key from the store.
*   Each key can be of size no greater than 256B and each value can be of size no greater than 128KB. If the size is breached, return an error
*   When inserting a Key-Value pair, if the key already exists, then the value is overwritten. If the value is overwritten, then the "Status" field should have "True", else "False".
*   When retrieving an value, if the key does not exist, return an error message.
*   You should make sure that the Server works in parallel, for instance, if the server is blocked on KVStore for a PUT operation, that should not stop other requests from being executed. However, if multiple operations are being performed on the same entry (i.e. unique Key), the operations should be serialized. i.e., if a PUT (k, v) is followed by a GET (K), the GET should wait for the PUT to finish before returning.
*   For all networking parts of this Phase, you should use only the <tt>java.net.Socket</tt> and <tt>java.net.ServerSocket</tt> classes. You should not use any wrappers around the Socket class. If in doubt, post on Piazza if it is acceptable.
*   For this project, you cannot use any thread safe data structures that has been defined by the JVM. For example, you will have to use Conditional Variables and Locks with a <tt>java.util.LinkedList</tt> rather than depend upon Java's synchronized implementations (such as <tt>java.util.concurrent.BlockingQueue</tt>). We want you to learn how to build thread safe data structures by using the basic synchronization building blocks (Locks, ReadWriteLocks, Conditional Variables, etc) that you learnt in Projects 1 and 2. This means that you can use the synchronized keyword, locks (including readwrite locks), java object 's internal locking and condition mechanisms, non thread safe data structures like HashMap and LinkedList.
*   You should ensure the following synchronization properties in your Key-Value service: 
    1.  reads (GETs) and updates (PUTs and DELETEs) are atomic.
    2.  An update consists of modifying a (key, value) entry in both the KVCache and KVStore.
    3.  an update cannot overlap with a read or another update.
    4.  In order to provide adequate parallelism, multiple reads CAN overlap (subject to property 3)
    *   You should bullet proof your code, such that the Key-Value server does not crash under any circumstances.
    *   For your final submission, you will submit your code and host your applications (part 1 and part 5) on an EC2 instance. You will submit the access details for the instance along with your code.
    *   You will run the Key-Value service on port 8080.</ul> 
### Tasks (weightage / approximate lines of code):

1.  *(10% / 15 loc)* Set up your EC2 accounts, launch a machine, log into the machine. Create a program called <tt>PingPong</tt> that uses TCP Sockets to listen for connections on port 8081. The program should service each incoming TCP request on the socket with the string 'pong' (without the quotes).
2.  *(30% / 150 loc)* Implement the <tt>KVClient</tt> class that you are provided with, such that it will issue requests and parse responses in the appropriate format. You will implement the message parsing library in <tt>KVMessage</tt>. The client is responsible for marshalling and unmarshalling the keys and values.
3.  *(15% / 50 loc)* Implement a *threadpool* -- you are not allowed to use existing implementation of threadpools. The thread pool should accept different tasks and execute them asynchronously. The threadpool should maintain a queue of tasks submitted to it, and should assign it to a free thread as soon as it is available. Ensure that the <tt>addToQueue</tt> interface to the threadpool is non-blocking.
4.  *(15% / 50 loc)* Implement a thread safe fully-associative LRU cache. The cache should be instantitated with a parameter that specifies the size of the cache.
5.  *(30% / 150 loc)* Implement the Key-Value server which uses the threadpool to parallelize data storage into the dummy storage system provided to you (<tt>KVStore</tt>). Use the LRU Cache you implemented to make key lookups faster. You should follow a **write-through** caching policy.

### Your Key-Value server should support the GET/PUT/DELETE interface with the following format:

*   **Get Value Request:**  
    <?xml version="1.0" encoding="UTF-8"?>  
    <KVMessage type="getreq">  
    <Key>key</Key>  
    </KVMessage>  
    
*   **Put Value Request:**  
    <?xml version="1.0" encoding="UTF-8"?>  
    <KVMessage type="putreq">  
    <Key>key</Key>  
    <Value>value</Value>  
    </KVMessage>  
    
*   **Delete Value Request:**  
    <?xml version="1.0" encoding="UTF-8"?>  
    <KVMessage type="delreq">  
    <Key>key</Key>  
    </KVMessage>  
    
*   **Successful Get Response:**  
    <?xml version="1.0" encoding="UTF-8"?>  
    <KVMessage type="resp">  
    <Key>key</Key>  
    <Value>value</Value>  
    </KVMessage>
*   **Successful Put Response:**  
    <?xml version="1.0" encoding="UTF-8"?>  
    <KVMessage type="resp">  
    <Status>True/False</Status>  
    <Message>Success</Message>  
    </KVMessage>
*   **Successful Delete Response:**  
    <?xml version="1.0" encoding="UTF-8"?>  
    <KVMessage type="resp">  
    <Message>Success</Message>  
    </KVMessage>
*   **Unsuccessful Get/Put/Delete Response:**  
    <?xml version="1.0" encoding="UTF-8"?>  
    <KVMessage type="resp">  
    <Message>Error Message</Message>  
    </KVMessage>

### Possible Error Messages (error messages are case sensitive):

*   "Success" -- There were no errors
*   "Network Error: Could not send data" -- If there is an error sending data
*   "Network Error: Could not receive data" -- If there is an error receiving data
*   "Network Error: Could not connect" -- Could not connect to the server, port tuple
*   "Network Error: Could not create socket" -- Error creating a socket
*   "XML Eror: Received unparseable message" -- Received a malformed message
*   "Over sized value" -- In the case that the submitted value is over 128KB
*   "Over sized key" -- In the case that the submitted key is over 256B (does not apply for get requests)
*   "IO Error" -- If there was an error raised by <tt>KVStore</tt>
*   "Does not exist" -- For GET/DELETE requests if the corresponding key does not already exist in the store
*   "Unknown Error: error-description" -- For any other error

 [1]: pics/proj3-arch.png
 [2]: https://github.com/CS162Berkeley/Project3Skeletion/zipball/master
 [3]: http://inst.eecs.berkeley.edu/~cs162/sp12/ec2-spec.pdf
 
## Copyright 2012 UC Berkeley
##
## Keywords: CS162, Key Value Store





