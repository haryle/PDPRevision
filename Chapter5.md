## 1. Meta difference between MPI, OpenMP and Pthreads

MPI - for distributed memory system - is a library 

Pthread - for shared memory system - is a library 

OpenMP - for shared memory system - is a library. Allows for high level parallel code writing hence relies more on compiler support. 

MPI and Pthread work with any compiler as long as the library is linked. OpenMP on the other hand may not work if compiler does not support openMP.

## 2. How does omp specifies that the next block of code is to be executed in parallel

```
#pragma omp parallel num_threads(thread_count){
    //Code block
}
```

## 3. Does omp have an implicit barrier when the code block in omp parallel is completed? 

Yes 

## 4. OMP for critical section 

Use `pragma omp critical`

## 5. How does `reduction` clause work in OpenMP? 

```
global_result = 0.0;
# pragma omp parallel num threads(thread count) \
reduction(+: global_result)
global_result += Local_trap(double a, double b, int n);
```

Omp creates private variable global result for each thread, which calculates area using `Local_trap` function. Thus the call to  `Local_trap` can be done in parallel. Omp also creates a critical section for adding the local variables to the global variable `global_result`.

## 6. How does the `parallel for` directive work? 

The following code block must be a for loop. OMP will split the for loop among its threads, based on block scheduling by default, and static for cyclic scheduling. 

## 8. Is the running index in parallel for shared? 

Loop variable i is shared (the running index)

## 7. Outline different ways to do trapezoidal integration with omp: 

- With explicit critical section 

```
// global_sum, thread_count, a, b, n are in default scope
# pragma omp parallel num_threads(thread_count){
    double my_sum = Local_Trap(a, b, n);
    # pragma critical
    global_sum = global_sum + my_sum;
}
```

- With reduction clause 
  
```
// global_sum, thread_count, a, b, n are in default scope
# pragma omp parallel num_threads(thread_count)\
    reduction(+:global_sum)
    global_sum += Local_trap(a,b,n);
```

- With parallel for

```
h = (b-a)/n; 
global_sum = (f(a) + f(b))/2.0; 
#   pragma omp parallel for num_threads(thread_count) \
    reduction(+: global_sum)
    for (i = 1; i <= n-1; i++)
        global_sum += f(a + i*h);
    // Implicit barrier here 
    global_sum = h*global_sum; 
```

## 8. Describe data dependences 

When result of one or more iterations depend on other iterations. 

## 9. What is the difference between omp parallel for and omp for 

Whenever omp parallel for is called, it forks new threads and joins old threads which create overhead costs. omp for on the other hand, uses threads forked from the enclosing omp block, hence is more efficient. 

## 10. Does omp for have an implicit barrier? 

Yes 