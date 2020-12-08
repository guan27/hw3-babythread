# System Programming 2020: BabyThread
## 0. The Goal
0. Understand and simulate the concept of context switch.
1. Understand and learn some system calls about signals.
2. Understand and learn to use `setjmp()`, `longjmp()`.
3. Get points to pass this course :)

## 1. Problem Description
In this assignment, you need to simulate a user-thread library by using `longjmp()`, `setjmp()`, etc. For simplicity, we use a function to represent a thread. In other words, you'll have to "context switch" between functions. To do this, you need to do non-local jumps between functions, which is arranged by a `scheduler()`: each time a function needs to "context switch" to another, it needs to jump back to `scheduler()`, and `scheduler()` will schedule next function to be executed, thus jump to it. Since non-local jump won't store local variables, we define a data structure for each function to store data needed for computing, which is called `TCB_NODE`. All the `TCB_NODE`s will formulate a circular linked-list. As `scheduler()` schedules functions, it also needs to make sure `Current` pointer points to correct `TCB_NODE` for functions to output correct result.  
The "context switch" mentioned above may occur in three different scenarios: signal caught, timeslice reached, or after each iteration. we'll introduce the way to choose between three different scenarios in **6. Execution**. 
  
You are expected to complete the following tasks:  
0. Complete the user-thread library `babythread.h`.
1. Complete the functions required in `utils.c`.
2. Implement three functions: `BlackholeNumber()`, `BinarySearch()`, and `FibonacciSequence()` in `threefunctions.c`.

## 2. main.c
You **DON'T** have to change any code in `main.c` :) But for you to understand the program's workflow, we provided the source code of `main.c`. You should done all your homework **WITHOUT** changing any variables or insert any code segement in `main.c`.

## 3. babythread.h
`babythread.h` defines the structure of `TCB_NODE`, and all the global variables you needed for implementing `utils.c` and `threefunctions.c`, but `babythread.h` is lack of some macro's implementation. You job is to finish them. For more information, please check the comments in `babythread.h`.
## 4. utils.c
You need to implement `sighandler()` and `scheduler()` in `utils.c`, where `sighandler()` is "the sighandler" required in `sigaction()` system call and `scheduler()` is introduced in **1. Problem Description**. For more information, please check the comments in `utils.c`.

## 5. threefunctions.c
0. FunctionName refers to BlackholeNumber, BinarySearch,or FibonacciSequence.
1. Functions are all in the form mentioned below. (So you should write your function by adding code at comment segements only.)
2. Functions should atleast print an outputline during its timeslice.
3. Functions will only context switch at the end of each iteration.
4. Functions will teriminate on two condition: maxiter exceeded or produced required output.  
5. The output of each iteration and the required output of each function are:  
`BlackholeNumber()` : start from initial value(100-999), update the value by URL mentioned below then print. The required output is 495.  
                      (https://zh.wikipedia.org/wiki/黑洞數)  
`BinarySearch()` : start from 50, conduct binary search between range(0,100). You only have to do one step of binary search in each iteration. The required output is the initial value send to this function.  
`FibonacciSequence()` : print one entry of the Fibonacci Sequence (1,2,3,5,8,13,...) in each iteration. There's no required output for this function.  
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
In each timeslice, functions should output its current result by:
```
printf("FunctionName: %d\n", your_output);
```

## 6. Execution
This homework will only be executed by following command:
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
sample execution

## 7. Grading
0. (1 pt)Your `babythread.h` and `utils.c` supports context switch by iteration count.
1. (2 pt)Your `babythread.h` and `utils.c` supports context switch by time slice.
2. (2 pt)Your `babythread.h` and `utils.c` supports context switch by signal caught.
3. (2 pt)Your `threefunctions.c` works correctly.
4. (1 pt)Your `babythread.h`, `utils.c` and `threefunctions.c` work fine together.  

For all tasks, your code will be complied by `MakeFile` in this repostiory.  
- In 1. 2. 3., your `babythread.h` and `utils.c` will complied with TA's `main.c` and `threefunctions.c`.
- In 4., your `threefunction.c` will complied with TA's `main.c` and `babythread.h` and `utils.c`.
- In 5., your `babythread.h`, `utils.c` and `threefunctions.c` will be compiled with TA's `main.c`.
- TA's `threefunctions.c` is pre-compiled as `threefunctions.o` in this repository, use it well.

## 8. testdata.txt
Each line in `testdata.txt` is a command line input in the form states in **5. Execution**.
FYI there will be no private dataset in this homework, so all your grading will be based on `testdata.txt`.

## 9. Submission
Your assignment should be submitted to github before deadline. The submisstion should only include two files:
0. `babythread.h`
1. `utils.c`
2. `threefunctions.c`  
Your repository may contain other files, but TA will **ONLY** score your homework based on two files mentioned above, please make sure that you correctly named your code.

## 10. Reminder
0. Plagiarism is **STRICTLY** prohibited.
1. Your credits will be deducted 10% for each day delay. A late submission is better than absence.
2. If you have any question, feel free to contact us via email ntucsiesp@gmail.com or come during TA hours.
3. Please start your work as soon as possible, **DON'T** leave it until the last day!






