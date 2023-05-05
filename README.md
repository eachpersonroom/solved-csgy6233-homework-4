Download Link: https://assignmentchef.com/product/solved-csgy6233-homework-4
<br>
In this assignment, you will take a non thread-safe version of a hash table and modify it so that it correctly supports running with multiple threads. This does not involve xv6; xv6 doesn’t currently support multiple threads of execution, and while it is possible to do parallel programming with processes, it’s tricky to arrange access to some shared resources. Instead you will work on this assignment through Anubis’s IDE which uses a multicore machine.




Start by downloading the attached file, parallel_hashtable.c to your local machine and you will compile it with the following command:




$ gcc -pthread parallel_hashtable.c -o parallel_hashtable




Now run it with one thread:







<sub>         </sub>$ ./parallel_hashtable 1

[main] Inserted 100000 keys in 0.006545 seconds <sub> </sub>[thread 0] 0 keys lost!

[main] Retrieved 100000/100000 keys in 4.028568 seconds




So with one thread the program is correct. But now try it with more than one thread:




<sub>         </sub>$ ./parallel_hashtable 8

[main] Inserted100000 keys in 0.002476 seconds[thread 7] 4304keys lost!

[thread 6] 4464keys lost!

[thread 2] 4273keys lost!

[thread 1] 3864keys lost![thread 4]4085keys lost!

[thread 5]4391keys lost!

[thread 3]4554keys lost!

<sub> </sub>[thread 0]4431keys lost!

[main] Retrieved 65634/100000 keys in 0.792488 seconds




Play around with the number of threads. You should see that, in general, the program gets faster as you add more threads up until a certain point. However, sometimes items that get added to the hash table also get lost.




<h2>Part 1</h2>




Find out under what circumstances entries can get lost. Update parallel_hashtable.c so that insert and retrieve do not lose items when running from multiple threads. Verify that you can now run multiple threads without losing any keys. Compare the speedup of multiple threads to the version that uses no mutex – you should see that there is some overhead to adding a mutex.




You will probably need:




pthread_mutex_t lock; // declare a lock pthread_mutex_init(&amp;lock, NULL); // initialize the lock pthread_mutex_lock(&amp;lock); // acquire lock pthread_mutex_unlock(&amp;lock); // release lock




Once you have a solution to this problem save it to a file called parallel mutex.c.




Hint: You can also use man to get more documentation on any of these commands.

Alternatively, you can look them up the command online (<u>https://linux.die.net/man/</u>)




<h2>Part 2</h2>




Make a copy of parallel_mutex.c and call it parallel_spin.c.  Replace all of the mutex  APIs with the spinlock APIs in pthreads. The spinlock APIs in pthreads are:




<sub> </sub>pthread_spinlock_t spinlock; <sub> </sub>pthread_spin_init(&amp;spinlock, 0); <sub> </sub>pthread_spin_lock(&amp;spinlock);pthread_spin_unlock(&amp;spinlock);




Do you see a change in the timing? Did you expect that? Write down the timing differences and your thoughts in a comment in your source file or submit them in a text document on Brightspace.




<h2>Part 3</h2>




For Part 3 continue working with parallel_mutex.c. Does retrieving an item from the hash table require a lock? Update the code so that multiple retrieve operations can run in parallel.




<h2>Part 4</h2>




For Part 4 continue working with parallel_mutex.c. Update the code so that some insert operations can run in parallel.