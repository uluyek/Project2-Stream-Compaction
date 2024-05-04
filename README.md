CUDA Stream Compaction
======================

**University of Pennsylvania, CIS 565: GPU Programming and Architecture, Project 2**

* Keyu Lu
* Tested on: Windows 10, Dell Oman, NVIDIA GeForce RTX 2060

### Overview 
**My Implementations:**
 - Part 1 : implement cpu version of `StreamCompaction::CPU::scan`,`StreamCompaction::CPU::compactWithoutScan`,`StreamCompaction::CPU::compactWithScan`
 - Part 2 : naive gpu scan algorithm
 - Part 3 :  Work-Efficient GPU Scan & Stream Compaction
 - Part 4 :  Using Thrust's Implementation
 - Part 5 : explain the reason "Why is My GPU Approach So Slow?" (Extra Credit)
 - Part 6 : Radix Sort  (cpu version of Radix Sort) (Extra Credit)
 - Part 7 : Optimize the code so as to avoid warp divergence  (Extra Credit)
   
   **Note**
 - For the code of Part 7, I added two files `hard.cu` and `hard.h`

### Performance Analysis 
#### Performance test
set the array size [1<<10, 1<<12, 1<<14, 1<<16, 1<<18, 1<<20, 1<<22 ] to test 

 - the performance of the code for scan:
![](https://github.com/uluyek/Project2-Stream-Compaction/blob/main/scan.jpg)
 - the performance of the code for stream compaction:
![](https://github.com/uluyek/Project2-Stream-Compaction/blob/main/stream.jpg)

#### Profile
we use Nsight to profile the code, the profile figure is given below:
![](https://github.com/uluyek/Project2-Stream-Compaction/blob/main/nsight%20profile.jpg)

#### Performance Analysis

The performance was evaluated under various implementations:

- **CPU Version:** Most efficient with smaller datasets due to minimal overhead compared to GPU memory transfers.
- **Naive vs Efficient GPU:** The efficient version outperforms the naive one. However, due to single-block synchronization and warp divergence, it is still slower than the CPU version.
- **Part 7 - Hardware Optimized:** Nearly matches the performance of the Thrust version but is slightly slower. This discrepancy is primarily due to the single thread block used in our implementation versus multiple blocks in Thrust.
- **Optimization Opportunity:** Converting loops within kernels into multiple kernels could allow using multiple blocks, potentially matching or surpassing Thrust performance.

#### Issues with Up-Down Phase Prefix Scan

During the up-phase of the prefix scan, each thread corresponds to a specific index in the array, resulting in significant underutilization of threads. Initially, operations are performed on pairs like positions 0 and 1, 2 and 3, 4 and 5, leading to only half of the threads being active. As the scan progresses, fewer threads participate, culminating in severe warp divergence.

#### Optimization in Part 7 -- Adjusted Thread Indexing
In Part 7, we revised the thread indexing strategy to enhance utilization:
- **First Pass:** Thread indices 0 to 3 correspond to array positions 1, 3, 5, and 7.
- **Second Pass:** The same threads map to positions 3, 7, 11, and 15.
- **Mapping Formula:** `p = i * (2^p) - 1`, where `i` is the thread index and `p` is the number of scans.

This method ensures all threads are active, reducing idle time significantly.

### output (array size 2^22)
```
****************
** SCAN TESTS **
****************
    [  41  23  34  46  30  27  10   8  18   9  43  34  13 ...  30   0 ]
==== cpu scan, power-of-two ====
   elapsed time: 5.8957ms    (std::chrono Measured)
    [  41  64  98 144 174 201 211 219 237 246 289 323 336 ... 102714549 102714549 ]
==== cpu scan, non-power-of-two ====
   elapsed time: 5.875ms    (std::chrono Measured)
    [  41  64  98 144 174 201 211 219 237 246 289 323 336 ... 102714441 102714490 ]
    passed
==== naive scan, power-of-two ====
   elapsed time: 127.275ms    (CUDA Measured)
    passed
==== naive scan, non-power-of-two ====
   elapsed time: 128.733ms    (CUDA Measured)
    passed
==== work-efficient scan, power-of-two ====
   elapsed time: 87.0237ms    (CUDA Measured)
    passed
==== work-efficient scan, non-power-of-two ====
   elapsed time: 87.1689ms    (CUDA Measured)
    passed
==== work-Hard scan, power-of-two ====
   elapsed time: 12.4251ms    (CUDA Measured)
    passed
==== work-Hard scan, non-power-of-two ====
   elapsed time: 12.6988ms    (CUDA Measured)
    passed
==== thrust scan, power-of-two ====
   elapsed time: 6.19622ms    (CUDA Measured)
    passed
==== thrust scan, non-power-of-two ====
   elapsed time: 4.19597ms    (CUDA Measured)
    passed

*****************************
** STREAM COMPACTION TESTS **
*****************************
    [   1   3   2   2   0   2   2   3   2   3   3   0   1 ...   2   0 ]
==== cpu compact without scan, power-of-two ====
   elapsed time: 6.8114ms    (std::chrono Measured)
    [   1   3   2   2   2   2   3   2   3   3   1   1   3 ...   1   1 ]
    passed
==== cpu compact without scan, non-power-of-two ====
   elapsed time: 6.8995ms    (std::chrono Measured)
    [   1   3   2   2   2   2   3   2   3   3   1   1   3 ...   2   1 ]
    passed
==== cpu compact with scan ====
   elapsed time: 19.6372ms    (std::chrono Measured)
    [   1   3   2   2   2   2   3   2   3   3   1   1   3 ...   1   1 ]
    passed
==== work-efficient compact, power-of-two ====
   elapsed time: 86.7317ms    (CUDA Measured)
    passed
==== work-efficient compact, non-power-of-two ====
   elapsed time: 86.2505ms    (CUDA Measured)
    passed
==== cpu radix sort ***********
 ====
Passed
   elapsed time: 30.234ms    (CPU Measured)
```
