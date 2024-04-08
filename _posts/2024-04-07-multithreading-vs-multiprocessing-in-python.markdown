---
layout: post
title: "Multithreading vs Multiprocessing in Python, and when to use which!"
date: 2024-04-06 22:00:00 +0530
description: "it is exactly what the title says..."
categories: [ tech, operating_system, multiprocessing, python, multithreading ]
image: ../assets/post_imgs/2024-04-07-multithreading-vs-multiprocessing-in-python/multi_thumbnail.png
tags: [sticky]
author: raghav
---

[io_perf]: ../assets/post_imgs/2024-04-07-multithreading-vs-multiprocessing-in-python/io_perf.png
[cpu_perf]: ../assets/post_imgs/2024-04-07-multithreading-vs-multiprocessing-in-python/cpu_perf.png
[lru_cache]: ../assets/post_imgs/2024-04-07-multithreading-vs-multiprocessing-in-python/multi_thumbnail.png

In my career as a software engineer I’ve encountered multiprocessing and multithreading numerous times. I’ve seen many engineers struggle to realize the importance of using them. More importantly, I’ve noticed engineers not being able to decide **when to use which!** There’s more to it — they are a bit different when it comes to Python, all because of **GIL**.

> The Global Interpreter Lock (GIL) restricts execution of multiple threads in the same process to ensure that only one thread executes Python bytecode at a time. 

Notice, how this restriction applies to the threads of the _same process only_. This effectively limits the parallelism of multithreading in Python when it comes to CPU-bound tasks, as only one thread can execute Python code at a time. However, for I/O-bound tasks, where a significant amount of time is spent waiting for input/output operations (such as network requests, disk I/O, etc.), the GIL doesn’t pose as much of a bottleneck, making multithreading a viable option. This is essentially because, once a thread has made that I/O heavy request to network or disk, etc it’s just sitting idle. It does not require anymore CPU cycles. Hence, Python hands over those cycles to the other threads in the process.

On the other hand, multiprocessing bypasses the GIL by creating separate processes, each with its own instance of the Python interpreter. This allows true parallelism on multi-core systems, making multiprocessing more suitable for CPU-bound tasks that require heavy computation.

Let’s see our above understanding in action. Here is a sample code to demonstrate the difference between multithreading and multiprocessing for both I/O-bound and CPU-bound tasks.

First, let’s create a sample I/O-bound task:

```python
"""Program to show performance of multi-processing vs multi-threading 
for I/O Bound tasks"""

# Imports
import threading
import multiprocessing
import requests
import time

# Our sample URL which we'll use to generate an
# I/O bound task i.e. one that does not require CPU cycles
# and is dependent on Network call in this case
URL = 'https://jsonplaceholder.typicode.com/posts'

# Function to emulate a I/O bound task
def download_data(thread_id):
    """Sample function to download data response from
    our sample URL to act as proxy for an I/ bound network call"""

    response = requests.get(URL)
    print(f'Thread {thread_id}: Downloaded {len(response.content)} bytes')

# Function which downloads data
# using multi-threading
def io_bound_with_threads(num_threads):
    """To download data using multi-threading"""

    start_time = time.time()

    threads = []
    for i in range(num_threads):
        thread = threading.Thread(target=download_data, args=(i,))
        threads.append(thread)
        thread.start()

    # This makes sure that the code flow is blocked
    # till all threads finish their job
    for thread in threads:
        thread.join()

    end_time = time.time()
    print(f'Total time with {num_threads} threads: {end_time - start_time} seconds')


# Function which downloads data
# using multi-processing
def io_bound_with_processes(num_processes):
    """To download data using multi-processing"""

    start_time = time.time()

    processes = []
    for i in range(num_processes):
        process = multiprocessing.Process(target=download_data, args=(i,))
        processes.append(process)
        process.start()

    # This makes sure that the code flow is blocked
    # till all processes finish their job
    for process in processes:
        process.join()

    end_time = time.time()
    print(f'Total time with {num_processes} processes: {end_time - start_time} seconds')

# For driving the above performance experiment
if __name__ == "__main__":
    num_threads = num_processes = 10
    io_bound_with_threads(num_threads)
    io_bound_with_processes(num_processes)
```

And the output for the above code is:

![io_perf Example][io_perf]
*Output for the above experiment*

For I/O bound task multithreading performs better than multiprocessing.

Now, let’s create a sample CPU-bound task:

```python
"""Program to show performance of multi-processing vs multi-threading 
for CPU Bound tasks"""

# Imports
import multiprocessing
import threading
import time

# Our candidate function which will make sure that the
# task is CPU bound i.e. need CPU cycles
def fibonacci(n: int) -> int:
    """Well, well this is the standard 
    Fibonacci series function"""
    if n <= 2:
        return 1
    else:
        return fibonacci(n-1) + fibonacci(n-2)

# Function which finds fibonacci number
# using multi-threading
def cpu_bound_with_threads(num_threads: int):
    """To find fibonacci using multi-threading"""

    start_time = time.time()

    threads = []
    for i in range(num_threads):
        thread = threading.Thread(target=fibonacci, args=(35,))
        threads.append(thread)
        thread.start()
    
    # This makes sure that the code flow is blocked
    # till all threads finish their job
    for thread in threads:
        thread.join()

    end_time = time.time()
    print(f'Total time with {num_threads} threads: {end_time - start_time} seconds')

# Function which finds fibonacci number
# using multi-processing
def cpu_bound_with_processes(num_processes):
    """To find fibonacci using multi-processing"""

    start_time = time.time()

    processes = []
    for i in range(num_processes):
        process = multiprocessing.Process(target=fibonacci, args=(35,))
        processes.append(process)
        process.start()

    # This makes sure that the code flow is blocked
    # till all processes finish their job
    for process in processes:
        process.join()

    end_time = time.time()
    print(f'Total time with {num_processes} processes: {end_time - start_time} seconds')

# For driving the above performance experiment
if __name__ == "__main__":
    num_threads = num_processes = 10
    cpu_bound_with_threads(num_threads)
    cpu_bound_with_processes(num_processes)
```

And the output for the above code is:

![cpu_perf Example][cpu_perf]
*Output for the above experiment*

For CPU bound task multiprocessing performs better than multithreading.

So at this point of time, it can be assumed generally, that multithreading is more suited for I/O heavy tasks and multiprocessing for CPU heavy tasks. There are quite a few scenarios where this might not be entirely true, and there will be occasions when one’ll need to use the combination of the two to achieve optimality.

Thank you for reading the article! I hope you found the answers you were looking for. Until the next time!

<br>
<br>
<br>
> शक्नोतीहैव यः सोढुं प्राक्शरीरविमोक्षणात्‌ ।            
> कामक्रोधोद्भवं वेगं स युक्तः स सुखी नरः ॥          
> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;-- Bhagavad Gita 5.23 ॥

(Read about this Shloka from the Bhagavad Gita here at [sanskritslokas.com](http://sanskritslokas.com/gita-slokas1.html))