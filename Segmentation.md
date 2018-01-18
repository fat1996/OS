<b>1. What's a segment?</b>
A segment is just a contiguous portion of the address space of a particular length.

<b>2. What's segmentation?</b>
In the address space, we have 3 diff segments i.e code, stack, heap.
What segmentation allows the OS to do is to place each one of those segments in different parts of physical memory, and thus avoid filling physical memory with unused virtual address space.

<b>Demonstration:</b>
Suppose we have the following address space:
Segment  Base  Size
1. Code   32K  2K
2. Heap   34K  2K
3. Stack  28K  2K

<b>Sample address translation:</b>
1. Assume a reference is made to virtual address 100 (which is in the code segment). 2. When the reference takes place (say, on an instruction fetch), the hardware will add the base value to the offset into this segment (100 in
this case) to arrive at the desired physical address: 100 + 32KB, or 32868.
3. It will then check that the address is within bounds (100 is less than 2KB),
find that it is, and issue the reference to physical memory address 32868.

<b>External Fragmentation:</b>
The general problem that arises is that physical memory quickly becomes full of little holes of free space, making it difficult to allocate new segments, or to grow existing ones. 
For e.g., say we get a request for 20KB. We have 24KB free, but not in 1 contiguous segment. Thus, the OS cannot satisfy the 20KB request.

<b>How to resolve these problems?</b>
The OS could <b>compact</b> the physical memory by rearranging the existing segments. 
For example, the OS could stop whichever processes are running, copy their data to one contiguous region of memory, change their segment register values to point to the new physical locations, and thus have a large free extent of memory with which to work. 
By doing so, the OS enables the new allocation request to succeed. 
However, compaction is expensive, as copying segments is memory-intensive and generally uses a fair amount of processor time.

There's tonnes of other algorithms such as best-fit, worst-fit, first-fit etc, but external fragmentation will still exist, and a good algorithm will only minimize it.





