# InterThreadCommunication in C/C++. How can we control/schedule execution of threads in C, C++? 

First two examples are in C and last one is in C++. In my first approach(ThreadControl.c) I am using 3 mutexs and 3 condition variables. With the examples in my project folder,  you can schedule or control any number of threads in C and C++. First look at the first thread example(ThreadControl.c). Here it locked mutex lock1  (so that other thread could not access the codes) starts executing (codes not added just comments) and  finally after completing its task waiting on cond1, likewise second thread locked mutex lock2, starts executing  its business logic  and finally waits on condition cond2 and 3rd thread locked mutex lock3, starts executing its business logic and finally waits on condition cond3. I am not adding any business logic here because this is just an example. In the commented section you can add your business logic which will execute  in parallel mode. Suppose thread3 depends on final output of thread1 which is going to be inserted in a table and thread3 will read that information before creating it final result and thread2 depends on final outcome of thread3 to generate its final outcome. Hence thread1 after inserting the data into table, signals thread3 through condition variable to go ahead with its final process. That means thread1 controls thread3. As thread2 depends on final outcome from thread3, hence thread3  controls  the execution of Thread2. Here we can allow thread1 to execute independently as its operation does not depends on any other thread, but for example of thread control we are controlling all the threads here and hence thread1 is being controlled from thread2. 

To start the controlling process, we are releasing thread1 first. In the main thread (i.e. main function, every program has one main thread, in C/C++ this main thread is created automatically by operating system once the control pass to the main method/function by kernel) we are calling pthread_cond_signal(&cond1); Once this function called from main thread,  thread1 which was waiting on cond1 will be released and it will start executing further. Once it finishes  with its final task, it will call pthread_cond_signal(&cond3); now thread which was waiting on condition cond3 i.e. thread3 will be released and it will start to execute it’s final stage and will call pthread_cond_signal(&cond2); and it will release the thread which is waiting on condition cond2 i.e. in this case thread2. This is the way we can schedule and control execution of thread in multi-threaded environment.

In my second approach (ThreadControl2.c), I am using global variable as controller to control threads. Please see example (ThreadControl2.c) carefully how it has been scheduled/controlled based on a global variable. But best approach is the first example, ThreadControl2.c is just for understanding. Here I don’t need to explain the logic. Just check the “if condition“ inside while loop, you will understand it very easily.

Now my third example is in C++( ThreadControl.cpp). Here I am using the same approach as I have applied in first C example. If you directly come to this example please read my very first example in C to understand the approach. In the example in ThreadControl.cpp, I have developed the code in Visual Studio 2013 and I am not distributing the codes into separate header and cpp files. Also declared and defined all the method inline as this is simply an example only. Also you might thing that why I am declaring 3 classes while class structure is same. Don’t be confused, for example only I am using same class definition with three different name. Look at the commented line for business logic. Here every class will have different business and functional logic. This example is for 3 different threads with 3 different classes; just assume that all the classes are different with different functionality and business logic.
