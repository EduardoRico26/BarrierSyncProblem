# ARSW - Laboratory #2

## Barrier Synchronization in Java

**Author:** Eduardo Rico Duarte

**Course:** Software Architectures (ARSW)

**Institution:** Escuela Colombiana de Ingeniería Julio Garavito

---

# Introduction

The purpose of this laboratory is to analyze and correct a synchronization problem in a concurrent Java application.

Through this exercise, fundamental concepts of concurrency and thread coordination are studied, as well as the importance of synchronization mechanisms in concurrent applications.

---

# Theoretical Framework

Concurrency allows multiple tasks to be executed simultaneously through the use of *Threads*. In Java, threads share memory and can work in parallel, improving application performance.

When several tasks execute concurrently, synchronization mechanisms are required to coordinate their execution and avoid incorrect results. One such strategy is barrier synchronization, which ensures that a group of threads reaches a specific execution point before another process can continue.

In this laboratory, the `join()` method is used to make the main thread wait for all worker threads to finish before calculating the average execution time, ensuring that the obtained results are correct.

---

# Laboratory Development

## Point 2. Review and Execution of the Main Program

### Objective

Execute the provided program, observe its behavior, and analyze whether the calculation of the average execution time is correct.

### Development

The program creates 20 threads (`HiloProc`), each with a random waiting time. Each thread executes a task consisting of 10 iterations and records its total execution time in the `resultado` variable.

When the program was executed, the following output was obtained:

![alt text](Imagenes/EV1.png)

.
.
.

![alt text](Imagenes/Ev2.png)

It was observed that the average execution time message appeared immediately after starting the threads, even before they began completing their tasks.

### Analysis

The obtained result is incorrect. The calculated average is equal to zero because the main thread performs the calculation immediately after invoking `start()` on the worker threads.

The `resultado` variable of each thread is initialized to zero and is only updated when the `run()` method finishes. Since the main thread does not wait for the worker threads to complete, the average is calculated using values that have not yet been updated.

### Conclusion

The program presents a synchronization problem. The average execution time is calculated before the threads complete their work, producing an incorrect result.

---

## Point 3. Applying a Barrier Synchronization Strategy

### Objective

Ensure that the average execution time is calculated only after all threads have completed their execution.

### Development

To solve the problem, Java's `join()` method was used. This method allows the main thread to wait for each worker thread to finish before continuing its execution.

The following code block was added after starting the threads:

```java
try {
    for (int i = 0; i < numHilos; i++) {
        hilos[i].join();
    }
} catch (InterruptedException e) {
    e.printStackTrace();
}
```

Once all threads finish, the program calculates the average using the actual execution times recorded by each thread.

### Solution Explanation

The `join()` method blocks the execution of the main thread until the corresponding thread finishes. By applying `join()` to every thread in the array, the program guarantees that the average calculation is not performed until all threads have completed their tasks.

This strategy produces the same expected effect as a synchronization barrier in this scenario, since it forces the main thread to wait for all participants to finish before continuing.

### Conclusion

The implemented synchronization mechanism eliminates the problem identified in the previous point and ensures that the average is calculated using valid and complete information.

---

## Point 4. Verification of the Solution

### Objective

Verify that the implemented synchronization strategy corrects the incorrect behavior of the program.

### Development

After applying synchronization through `join()`, the program was executed again.

During execution, the following behavior was observed:

1. The 20 threads perform their work concurrently.
2. The main thread remains blocked while the worker threads execute.
3. All threads complete their 10 iterations.
4. The average execution time message appears only at the end of the execution.

The output now presents the following behavior:

![alt text](Imagenes/Ev3.png)

...

![alt text](Imagenes/ev4.png)

### Analysis

The change confirms that the main thread correctly waits for all worker threads to finish before continuing.

When the calculation is performed, all instances of `HiloProc` have already updated their `resultado` variable, meaning that the calculated average corresponds to the actual execution time of the threads.

### Conclusion

The implemented solution works correctly and fulfills the objective of the laboratory. The average execution time is calculated only after all threads have completed their execution, eliminating the synchronization problem present in the original version of the program.

---

# General Conclusions

* Concurrency allows multiple tasks to be executed simultaneously through threads.
* Incorrect use of threads can lead to synchronization problems and inconsistent results.
* The `start()` method only initiates thread execution and does not guarantee its completion.
* Synchronization is necessary when an operation depends on results produced by multiple threads.
* The `join()` method allows coordination between the main thread and worker threads, ensuring that a task continues only after all others have finished.
* The implemented solution made it possible to obtain a valid and consistent average execution time.

---

# References

Benavides Navarro, L. D., & Gualtero Martínez, R. H. (2024). *Concurrency and Threads in Java and Go* [Course slides].

OpenAI. (2026). *ChatGPT (GPT-5.5 version)* [Large Language Model]. https://chatgpt.com/ (Used primarily as a support tool.)

Oracle. (2024). *Thread (Java Platform, Standard Edition 24 API Specification).* Oracle Corporation. https://docs.oracle.com/en/java/javase/24/docs/api/java.base/java/lang/Thread.html
