Download Link: https://assignmentchef.com/product/solved-comp-3500-lab-2-assignment
<br>
<strong>Instructions</strong> This lab assignment explores the performance of three memory management techniques: Best-Fit, WorstFit, and Paging.

<strong>Objectives of this assignment: </strong>

<ul>

 <li>to work on a Unix based system</li>

 <li>to “<em>dust off</em>” your programming skills in C</li>

 <li>to become familiar with the notion of a process control block</li>

 <li>to experience the life cycle of a process</li>

 <li>to “feel” how an operating system manages processes and memory</li>

 <li>to evaluate and compare three simple memory management strategies</li>

</ul>

<strong> </strong>

<strong>IMPORTANT:                                                  </strong>

<ul>

 <li><em>Your code will be tested and graded <strong>REMOTELY </strong>on the Engineering Unix (Tux) machines. If the code does not work on those machines, you will not get any credit even if your code works on any other machine. </em></li>

 <li><em>A late submission will get a 50% penalty if submitted right after the deadline. The next day, you cannot submit the lab. </em></li>

 <li><em>One submission per group. </em></li>

 <li><em>Writing and presentation of your report are considered to grade your lab (30%). Your conclusions <strong>must be supported</strong> by the data/measurements you collect. </em></li>

 <li><em>The quality of your code will be evaluated (20%). </em></li>

 <li><strong><em>Questions about this lab must be posted on Piazza if you need a timely answer</em></strong><em>. </em></li>

</ul>







<strong>Lab Assignment (Turned in by one group mate) </strong>

Note that the <strong><em>blue names in bold and Italic font</em></strong> will refer to variables you can access in the files <em>processesmanagement2.c</em> or <em>common2.h</em>.

Lab2 builds on Lab 1 described below. For Lab 1, memory was not a concern: it was assumed that memory was infinite. Lab 2 differs from Lab 1 by:

<ul>

 <li><strong>Memory for Lab 2 is finite</strong>. <strong><em>AvailableMemory </em></strong>is a variable contained in the file <em>h</em>. It is initialized by the system to some value. You cannot increase <strong><em>AvailableMemory </em></strong>beyond that initial value. You must accordingly decrease <strong><em>AvailableMemory </em></strong>as you allocate memory to processes and must increase it as you free the memory when these processes complete. <strong><em>AvailableMemory </em></strong>is defined in the file <strong><em>common2.h</em></strong><em>.</em> extern Memory AvailableMemory; // Total Available Memory</li>

 <li>Each process will be created with <strong>memory requirements</strong>: each process control block contains the information :</li>

</ul>

Memory TopOfMemory;      /* Address of top of allocated memory block */

Memory MemoryAllocated;  /* Amount of Allocated memory in bytes      */

Memory MemoryRequested;  /* Amount of requested memory in bytes      */

When a process is generated, the variable MemoryRequested will be set to some value that you cannot change. The system must find a free hole larger than  MemoryRequested for the process to run it.

<ul>

 <li>A process cannot be <em>admitted</em> if there is not enough memory to accommodate it. You must keep track of the address where the process is loaded (TopOfMemory) and you must keep track of the list of free memory holes to accommodate future requests.</li>

 <li>When a process completes, memory must accordingly be updated to eventually admit new processes.</li>

</ul>




<strong>Road Map to a Successful Lab 2 </strong>

In order to build progressively Lab2 and determine the bounds on the performance you can hope for, you must follow these steps:

<strong>Step 1:</strong> In addition to the metrics collected for lab 1 (the average turnaround time (<strong>TAT</strong>), the average response time (<strong>RT</strong>), the CPU Busy time (%) (<strong>CBT</strong>), the throughput (<strong>T</strong>), and the average waiting time (in <strong>Ready State</strong>) (<strong>AWT</strong>)),   you must collect also the Average Waiting Time in the Job Queue (<strong>AWTJQ</strong>) and the number of completed processes.  <em>AWTJQ</em> measures how fast you allocate memory to a process. The better is the memory policy, the lower <em>AWTJQ</em> should be (right?).  You can keep track of the time in the job queue by updating this information in the process control block:

TimePeriod TimeInJobQueue; //Total time process spent in job queue

The number of completed processes is a good clue of the performance of the memory allocation policy.

<strong>Step 2:</strong> Collect all metrics for FCFS, SRTF, and RR (Quantum = 10ms, 20ms, 50ms, 250ms, and 500ms) using the program <strong>processesmanagement2.c</strong> ASIS (i.e., as provided without any modification). This program assumes an infinite memory. Therefore, the values you will get establish the optimal values you can hope for if memory was infinite. Can you predict what  <em>AWTJQ</em> will be when memory is infinite?

<strong>Step 3: </strong>Implement the <em>Optimal Memory Allocation Policy</em><strong> (OMAP)</strong>. Do not worry: it is simple. Manage your memory without worrying about <strong>where </strong>in the memory you place the processes or <strong>where</strong> the free memory blocks are. Just manage the memory using the variable <strong><em>AvailableMemory</em></strong>. As long as (<strong><em>AvailableMemory &gt;= pcb-&gt;MemoryRequested</em></strong>), you can admit the process and accordingly decrease <strong><em>AvailableMemory</em></strong>. When the process completes, free the memory by increasing accordingly <strong><em>AvailableMemory</em></strong>. Collect all metrics for FCFS, SRTF, and RR (Quantum = 10ms, 20ms, 50ms, 250ms, and 500ms) using the program <strong>proccessesmanagement2.c</strong> that you just improved by managing <strong><em>AvailableMemory</em></strong>. The performance you will observe is the best performance you can hope for: it is good to know this <em>“upper”</em> bound.

<strong>Step 4: </strong>Implement the easiest memory allocation strategy. This strategy is ……. <strong>Paging</strong>!!! If you think about it: with paging, you do not have to worry about placing the processes <strong>contiguously</strong> or <strong>where</strong> you place those pages. In this case, all you need to do is to express the <strong><em>AvailableMemory</em></strong> in terms of

<strong><em>NumberOfAvailablePages</em></strong>   and      the       <strong><em>pcb-&gt;MemoryRequested     </em></strong>in         terms   of  <strong><em>NumberOfRequestedPages</em></strong>. Your implementation of Paging will look very much close to <strong>Optimal Memory Allocation Policy</strong>. For page sizes 256 and 8 KB (8192), collect all metrics for FCFS, SRTF, and RR (Quantum = 10ms, 20ms, 50ms, 250ms, and 500ms) using the program <strong><em>proccessesmanagement2.c</em></strong> that you just improved by managing <strong><em>AvailableMemory</em></strong> in terms of pages, rather than bytes. For your analysis, think to compare OMAP with paging with the two diferent page sizes: <em>OMAP</em> is simply paging with a page size of one byte.

<strong>Step 5: </strong>Implement<strong> Best-Fit </strong>memory allocation strategy. I suggest to use a double linked list to manage the free memory holes. Collect all metrics for FCFS, SRTF, and RR (Quantum = 10ms, 20ms, 50ms, 250ms, and 500ms) using the program <strong><em>proccessesmanagement2.c</em></strong> that you just augmented with the implementation of  <strong>Best-Fit</strong>.

<strong>Step 6: </strong>Redo step 5 with the <strong>Worst-Fit </strong>memory allocation strategy.

<strong>Step 7:</strong> Write a good report to report and analyze your results.


