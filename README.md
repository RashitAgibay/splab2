## splab2 - forking processes
###### \[deadline is week06 labtime, October 8-12, 2018 \]

### Aim
- To master process creation system calls

### Tasks
- solve several small tasks that use `fork`, `exec*`, `waitpid`, `exit` UNIX/Linux system calls.

## DOMATH
Write a program `domath.c` and compile it as `domath`. The program should accept **two** arguments, create **four** child processes and pass the arguments to the child processes. Each of the child processes should perfom an arithmetic operation over the arguments (like addition, subtraction, multiplication or division) as shown below. File `domath.c` should contain code of parent process and all its child processes.
#include <stdio.h>
#include <unistd.h>
#include <sys/types.h>

int calc(int *arr, int n) {
    int sum = 0;
    int multi=0;
    int division=0;
    int subtraction=0;
    for (i = 1; i < n; i += 2) {
        sum = sum + arr[i];
        multi=multi*arr[i];
        division= arr[n-1]/arr[n-6];
        subtraction=arr[n]-arr[n-2];
    }
    return sum;
}

int main(void) {
    int arr[] = { 1, 2, 3, 4, 5, 6, 7, 8, 9, 10 };
    int n = sizeof(arr) / sizeof(arr[0]);

    pid_t pid = fork();
    if (pid < 0) {
        perror("fork failed");
        return 1;
    }
    else if (pid == 0) {
        printf("I am the child process\n");

        int child_sum = calc(arr, n);
        exit(child_sum);
    }
    else if (pid == 1) {
        printf("I am the child process\n");

        int child_multi = multi(arr, n);
        exit(child_multi);
    }
    else if (pid == 2) {
        printf("I am the child process\n");

        int child_division = div(arr, n);
        exit(child_division);
    }
    else if (pid == 3) {
        printf("I am the child process\n");

        int child_subtraction = subtraction(arr, n);
        exit(child_subtraction);
    }
    else {
        printf("I am the parent process\n");

        int parent_sum = calc(arr, n);
        int parent_multi=multi(arr, n);
        int parent_division = div(arr, n);
        int parent_subtraction = subtraction(arr, n);

        int child_sum;
        if (wait(&child_sum) == -1) {
            perror("wait failed");
        }

        else if {
            printf("Sum by child: %d\n", child_sum);
            printf("Multiplication by child: %d\n",child_multi);
            printf("Division by child: %d\n",child_division);
            printf("Subtraction bu child :%d\n", child_subraction)
        }
        printf("Sum by parent: %d\n", parent_sum);
        printf("Multiplication by parent: %d\n",parent_multi);
        printf("Division by parent: %d\n",parent_division);
        printf("Subtraction by parent:%d\n", parent_subraction)
    }

    return 0;
}


```
$ gcc domath.c -o domath
$ ./domath 3 5
child1: 3+5=8
child3: 3*5=15
child2: 3-5=-2
child4: 3/5=0
parent: done.
```

## MATHDO
Write a program `mathdo.c` that will take the same arguments as in the previous task and create **four** child processes. Unlike the previous task this task does not want the parent process to contain code for all its child processes. Instead the parent process spawns child processes written in other programming languages. Let `python` script do addition, a `java` program do subtraction, `nodejs` script do multiplication and a `bash` script do division. (they may need to be installed to your system first)

```
$ gcc mathdo.c -o mathdo
$ ./mathdo 3 5
python: 3+5=8
java: 3-5=-2
nodejs: 3*5=15
bash: 3/5=0
parent: done.
```

## SHELL
Write a program `shell.c` that repeatedly (in a loop) prompts user for a command, reads the command and executes it in a child process. If a user prints `&` in the end of a command your program should run the command as a child process in the background. When user enters `exit` command your program should exit.

```
$ Enter your command: ls -a
.  ..  a.c  b.py  c.cpp  D.java
$ Enter your command: gnome-mines &
$ Enter your command: gnome-calculator -e 2+3
$ Enter your command: exit
```

## PSHELL
Task D is an extension of **SHELL**. Add a built-in command `showjobs` which will show pids of all background processes only if they are still running. Give `pshell.c` name to your program.
```
$ gcc pshell.c -o pshell
$ ./pshell
$ Enter your command: gnome-mines &
$ Enter your command: gnome-calculator -e 2+3 &
$ Enter your command: showjobs
1. [65123]
2. [65124]
$ Enter your command: exit
```

## \*DOMATH the Linux Way (not for Mac users)

This task is optional.

`fork` and `exec` system calls are UNIX-wide. Linux has it's own `clone` system call for creating processes (actually threads). Complete task **DOMATH** using `clone` system call.

## References

1. `man fork`
2. `man execve`
3. `man waitpid`
4. `man clone`
5. [OSTEP Chapter 5](http://pages.cs.wisc.edu/~remzi/OSTEP/cpu-api.pdf)
6. Chapters 6, 24-28 of Linux Programming Interface by Micheal Kerrisk
7. Chapter 5 of Linux System Programming by Robert Love
