[
![](../fetched_images/Screenshot 2025-01-29 123137.png)]
This is a continuation of article [Link]
```
Quick Recap: 

The volatile keyword ensures that a variable is never cached and is always read and written directly from / to main memory. This prevents the data inconsistency between threads, that occurs due to CPU level caching. This keyword cannot be used with classes or methods. It can only be used with variables.
```



With that, in this post we are focusing on simulate a running example, which shows us why we need to add volatile keyword on shared variables among multiple threads in java. 
Let see two codes with volatile and without volatile.Code Source : [Link]
### With Volatile
Code
|  1
 2
 3
 4
 5
 6
 7
 8
 9
10
11
12
13
14
15
16
17
18
19
20
21
22
23 |classTestVolatileextends Thread \{volatileboolean keepRunning =true;publicvoidrun\(\)\{long count =0;while\(keepRunning\)\{
            count\+\+;\}

        System.out.println\("Thread terminated."\+ count\);\}publicstaticvoidmain\(String\[\] args\)throws InterruptedException \{
        TestVolatile t =new TestVolatile\(\);
        t.start\(\);
        Thread.sleep\(1000\);
        System.out.println\("after sleeping in main"\);
        t.keepRunning=false;
        t.join\(\);
        System.out.println\("keepRunning set to "\+ t.keepRunning\);\}\} |
|---|---|
Output
| 1
2
3 |after sleeping in main
Thread terminated.4223253655
keepRunning set to false
 |
|---|---|
### Without Volatile
Code
|  1
 2
 3
 4
 5
 6
 7
 8
 9
10
11
12
13
14
15
16
17
18
19
20
21
22
23 |classTestVolatileextends Thread \{boolean keepRunning =true;publicvoidrun\(\)\{long count =0;while\(keepRunning\)\{
            count\+\+;\}

        System.out.println\("Thread terminated."\+ count\);\}publicstaticvoidmain\(String\[\] args\)throws InterruptedException \{
        TestVolatile t =new TestVolatile\(\);
        t.start\(\);
        Thread.sleep\(1000\);
        System.out.println\("after sleeping in main"\);
        t.keepRunning=false;
        t.join\(\);
        System.out.println\("keepRunning set to "\+ t.keepRunning\);\}\} |
|---|---|
Output \(Running indefinitely / Take long time to exit\)
| 1 |after sleeping in main
 |
|---|---|
### Memory Updates with volatile
The volatile keyword prevents caching at the CPU level by only allowing read and write operations from/to the main memory directly. So, the volatile keyword guarantees the visibility of the variable to all threads. When the main thread executes t.keepRunning = false;, the update is written to the main memory. Since worker threads read from main memory, these threads will see the updated value immediately, and the while loop will be stopped.
### Memory Updates without volatile
Without the volatile keyword, the worker thread may use the cached value while executing the program, and it will take some time to reflect the updated value changes in main memory to these caches. When the main thread executes t.keepRunning = false;,  the update is written to the main memory, will not be reflected in the local cache immediately. So, worker threads may not see this change immediately. Due to this, the while loop will run until synchronization happens between main memory and the cached memories. And if the cache value isn't synced with the main memory, there is a possibility that the thread may continue running the loop indefinitely.
### Summary
* With volatile: The write to t.keepRunning = false; happens in main memory and is visible to all threads immediately.
* Without volatile: The write to t.keepRunning = false; happens in main memory and is not visible to the threads until synchronization happens between main memory and the cache.

Happy Coding 🙌
