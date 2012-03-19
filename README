###  README --- Project3Skeleton/

In Phase 3 of your class, you will implement a Key-Value Store that runs on a single node on Amazon EC2. As in phases 1 and 2, you will submit an initial design document and a final design document apart from the code itself.

* Your KeyValue Server will support 3 interfaces:
** Value GET (Key k): Retrieves the key-value pair corresponding to the provided key.
** PUT(Key k, Value v): Inserts the key-value pair into the store.
** DEL(Key k): Removes the key-value pair corresponding to the provided key from the store.
* Each key can be of size no greater than 256B and each value can be of size no greater than 128KB. If the size is breached, return an error
* When inserting a Key-Value pair, if the key already exists, then the value is overwritten. If the value is overwritten, then the "Status" field should have "True", else "False".
* When retrieving an value, if the key does not exist, return an error message.
* For all networking parts of this Phase, you should use only the java.net.Socket and java.net.ServerSocket classes. You should not use any wrappers around the Socket class. If in doubt, post on Piazza if it is acceptable.
* For this project, you cannot use any thread safe data structures that has been defined by the JVM. For example, you will have to use Conditional Variables and Locks with a java.util.LinkedList rather than depend upon Java's synchronized implementations (such as java.util.concurrent.BlockingQueue).
* We will test your submissions on a Ubuntu 11.10 Linux distribution with Sun Java 6.26 (packages sun-java6-bin, sun-java6-jdk and sun-java6-jre).
* You should bullet proof your code, such that the Key-Value server does not crash under any circumstances.
* For your final submission, you will submit your code and host your applications (part 1 and part 5) on an EC2 instance. You will submit the access details for the instance along with your code.
* You will run the Key-Value service on port 8080.

Tasks:
1. (10% / 15 loc) Set up your EC2 accounts, launch a machine, log into the machine. Create a program called PingPong that uses TCP Sockets to listen for connections on port 8081. The program should service each incoming TCP request on the socket with the string 'pong' (without the quotes).
2. (30% / 150 loc) Implement the KVClient class that you are provided with, such that it will issue requests and parse responses in the appropriate format. You will implement the message parsing library in KVMessage. The client is responsible for marshalling and unmarshalling the keys and values.
3. (15% / 50 loc) Implement a threadpool -- you are not allowed to use existing implementation of threadpools. The thread pool should accept different tasks and execute them asynchronously. The threadpool should maintain a queue of tasks submitted to it, and should assign it to a free thread as soon as it is available. Ensure that the addToQueue interface to the threadpool is non-blocking.
4. (15% / 50 loc) Implement a fully-associative LRU cache. The cache should be instantitated with a parameter that specifies the size of the cache.
5. (30% / 150 loc) Implement the Key-Value server which uses the threadpool to parallelize data storage into the dummy storage system provided to you (KVStore). Use the LRU Cache you implemented to make key lookups faster. You should follow a write-through caching policy.

Message Formats:
* Get Value Request:
<?xml version="1.0" encoding="UTF-8"?>
<KVMessage type="getreq">
<Key>key</Key>
</KVMessage>
* Put Value Request:
<?xml version="1.0" encoding="UTF-8"?>
<KVMessage type="putreq">
<Key>key</Key>
<Value>value</Value>
</KVMessage>
* Delete Value Request:
<?xml version="1.0" encoding="UTF-8"?>
<KVMessage type="delreq">
<Key>key</Key>
</KVMessage>
* Successful Get Response:
<?xml version="1.0" encoding="UTF-8"?>
<KVMessage type="resp">
<Key>key</Key>
<Value>value</Value>
</KVMessage>
* Successful Put Response:
<?xml version="1.0" encoding="UTF-8"?>
<KVMessage type="resp">
<Status>True/False</Status>
<Message>Success</Message>
</KVMessage>
* Successful Delete Response:
<?xml version="1.0" encoding="UTF-8"?>
<KVMessage type="resp">
<Message>Success</Message>
</KVMessage>
* Unsuccessful Get/Put/Delete Response:
<?xml version="1.0" encoding="UTF-8"?>
<KVMessage type="resp">
<Message>Error Message</Message>
</KVMessage>

## Copyright 2012 UC Berkeley
##
## Keywords: CS162, Key Value Store





