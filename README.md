# System Programming 2020: BabyThread
## 0. The Goal
0. Understand and simulate the concept of context switch.
1. Understand and learn some system calls about signals.
2. Understand and learn to use setjmp(), longjmp().
3. Get points to pass this course :)

## 1. Problem Description
In this assignment, you need to simulate a user-thread library by using `longjmp()`, `setjmp()`, etc. For simplicity, we use a function to represent a thread. In other words, you'll have to "context switch" between functions. To do this, you need to do non-local jumps between functions, which is arranged by a `scheduler`: each time a function needs to "context switch" to another, it needs to jump back to `scheduler`, and `scheduler` will schedule next function to be executed, thus jump to it. Since non-local jump won't store local variables, we need another structure for each function to store data needed for computing,which is called `TCB_NODE`. All the `TCB_NODE` will formulate a circular linked-list. As `scheduler` schedules functions, it also needs to make sure `Current` pointer points to correct `TCB_NODE` for functions to output correct result.  
The "context switch" may occur in three different scenario: signal caught, timeslice reached, or after each iteration. we'll introduce the way to choose between three different scenarios in the execution part.  
  
You are expected to complete the following tasks:
1. Complete the user-thread library "babythread.h".
2. Implement three functions: BlackholeNumber, BinarySearch, FibonacciSequence.
## 2. IMPORTANT STUFF


## 2. main.c
You **DON'T** have to change any code in main.c :) But for you to understand the program's workflow, we provided the source code of main.c. You should done all your homework **WITHOUT** changing any variables or insert any code segement.


## 2. BabyThread.h
Please complete the half-done file babythread.h in this repository. Please view the file for features you need to implement.

## 3. Three Functions
0. Functions are all in the form mentioned below. (So you should write your function by adding code at comment segements only.)
1. Functions should atleast print a number during its timeslice.
2. Functions will only context switch at the end of each iteration.
3. Functions will teriminate on two condition: maxiter exceeded or produced required output.  
4. The output of each iteration and the required output of each function are:  
BlackholeNumber: start from initial value, update the value by URL mentioned below then print. The required output is 495.  
                 (https://zh.wikipedia.org/wiki/黑洞數)  
BinarySearch: start from 50, conduct binary search between range(0,100). You only have to do one step of binary search in each iteration. The required output is the initial value send to this function.  
FibonacciSequence: print one entry of the Fibonacci Sequence (1,2,3,5,8,13,...) in each iteration. There's no required output for this function.  
```
void FunctionName(int thread_id, int init, int maxiter)
{
	ThreadInit(thread_id, init, maxiter);
	/*
	Some initilization if needed.
	*/
	for (Current->i = 0; Current->i < Current->N; ++Current->i)
	{
		sleep(1);
		/*
		Do the computation, then output result.
		Call ThreadJoin() if the work is done.
		*/	
		ThreadYield();
	}
	ThreadJoin()
}

```
## . Execution

```
$ ./main {bi_init} {bi_maxiter} {bl_init} {bl_maxiter} {fi_init} {fi_maxiter} {timeslice} {switchmode}
```
Below are argument explainations:
```
bi_init = The initial value pass into BinarySearch
bi_maxiter = The max iteration for BinarySearch to process
bl_init = The initial value pass into BlackholeNumber
bl_maxiter = The max iteration for BlackholeNumber to process
fi_init = The initial value pass into FibonacciSequence
fi_maxiter = The max iteration for FibonacciSequence to process
timeslice = time limit for a function to process until next "context switch"
switchmode = 0 for "context switch" after each iteration; 1 for "context switch" due to signal caught and timeslice reached
```

## . Grading
TA will complie your with
1. (2 pt)Your thread library supports context switch by iteration count.
2. (2 pt)Your thread library supports context switch by time slice.
3. (2 pt)Your thread library supports context switch by signal caught.
4. (2 pt)You write functions (BlackholeNumber, BinarySearch, FibonacciSequence)on your own.


## . Submission

## 8. Reminder
1. Plagiarism is **STRICTLY** prohibited.
3. Your credits will be deducted 10% for each day delay. A late submission is better than absence.
4. If you have any question, feel free to contact us via email ntucsiesp@gmail.com or come during TA hours.
5. Please start your work as soon as possible, **DON'T** leave it until the last day!



## 2. Running the Sample Servers

The provided sample code can be compiled as simple servers. 

**Compile**
You should **write your own makefile** to compile the code.
You can use the provided command to produce execution files first.
```bash=
$ gcc server.c -o write_server
$ gcc server.c -D READ_SERVER -o read_server
```

**Run**
The server will be listening to {port}
```bash=
$ ./write_server {port}
$ ./read_server {port}
```

The sample server handled the part of socket programming, so you are able to connect to the sample server once you run it. Your client and server can communicate to each other.

Feel free to modify any part of the code as you need, or implement your own server.


## 3. Testing your Servers at Client Side

```shell=
telnet {hostname} {port}
```
If your server is running on linux1.csie.ntu.edu.tw, and listening to port 3333, your command on the client side will be `telnet linux1.csie.ntu.edu.tw 3333`

If you are running sample servers, try to type something on the client side. You will see some message from server.

## 4. About the Preorder Record File 

The servers should be able to access the file `preorderRecord`.

The file includes 20 consumer structures, each with a consumer id (range from 902001 to 902020), and the number of masks each consumer can order.
A consumer can order 10 adult masks and 10 children masks in total. 

```cpp=
typedef struct {
    int id; //902001-902020
    int adultMask; //set to 10 by default
    int childrenMask; //set to 10 by default
} preorderRecord
```



## 5. Sample input and output

**Read Server**

A client can check how many masks they can order. 
Once it connect to a read server, the terminal will show the following:

```shell
Please enter your id (to check how many masks you can order):
```

If you type a id (for example, `902001`) on the client side, the server shall reply:


```shell
You can order 10 adult mask(s) and 10 children mask(s).
```
and close the connection from client.


But, if someone else is ordering using the same id, the server shall reply:
```shell
Locked.
```
and close the connection from client.


**Write Server**

A consumer can make preorders. It will first show how many masks a consumer can order just like a read server, then ask for masktype and number of mask to preorder.

Once it connect to a write server, the terminal will show the following:
```shell
Please enter your id (to check how many masks you can order):
```
If you type an id (for example, `902001`) on the client side, the server shall reply the numbers, following by a prompt on the nextline.

```shell
You can order 10 adult mask(s) and 10 children mask(s).
Please enter the mask type (adult or children) and number of mask you would like to order:
```
If you type a command (for example, `adult 2`), the server shall modify the `preorderRecord` file and reply:
```
Pre-order for 902001 successed, 2 adult mask(s) ordered.
```
and close the connection from client.

But, if someone else is ordering using the same id, the server shall reply:
```shell
Locked.
```
and close the connection from client.



## 6. Grading
1. Finish the Makefile. (1 points)
    * The servers can be produced successfully by `make` command.
2. A read server handles requests correctly. (1 point)
    * There would be only one request at a time for the subtask.
3. A write server handles requests correctly. (1 point)
    * There would be only one request at a time for the subtask.
4. Two requests issued to a read server. (1 point)
5. Two requests issued to a write server. (1.5 points)
6. Requests issued to write servers (1 point)
7. Requests issued to read servers and write servers (1.5 points)

## 7. Submission
Your assignment should be submitted to github before deadline. The submisstion should include at least two files:
1. Makefile
2. server.c 
You can submit other .c, .h files, as long as they can be compiled to two executable files named read_server and write_server by Makefile.

You are encouraged to submit a readme.txt, where you can briefly state how do you finish your program and something valuable you want to explain. 


