1. What's a segment?
A segment is just a contiguous
portion of the address space of a particular length.

2. What's segmentation?
In the address space, we have 3 diff segments i.e code, stack, heap.
What segmentation allows the OS to do is to place each one
of those segments in different parts of physical memory, and thus avoid
filling physical memory with unused virtual address space.

Demonstration:
Segment  Base  Size
1. Code   32K  2K
2. Heap   34K  2K
3. Stack  28K  2K

Sample address translation:
1. Assume a reference is made to virtual address 100 (which is in the code
segment). When the reference takes place (say, on an instruction fetch),
the hardware will add the base value to the offset into this segment (100 in
this case) to arrive at the desired physical address: 100 + 32KB, or 32868.
It will then check that the address is within bounds (100 is less than 2KB),
find that it is, and issue the reference to physical memory address 32868.
2. Now let’s look at an address in the heap, virtual address 4200 (again
refer to Figure 16.1). If we just add the virtual address 4200 to the base
of the heap (34KB), we get a physical address of 39016, which is not the
correct physical address. What we need to first do is extract the offset into
the heap, i.e., which byte(s) in this segment the address refers to. Because
the heap starts at virtual address 4KB (4096), the offset of 4200 is actually
4200 minus 4096, or 104. We then take this offset (104) and add it to the
base register physical address (34K) to get the desired result: 34920.

<b>External Fragmentation:</b>
The general problem that arises is that physical memory quickly becomes
full of little holes of free space, making it difficult to allocate new
segments, or to grow existing ones. For e.g., say we get a request for 20KB. We have 24KB free, but not in 1 contiguous segment. Thus, the OS cannot satisfy the 20KB request.

<b>How to resolve these problems?</b>
1. The OS could <b>compact</b> the physical memory by rearranging the existing segments. For example, the OS could stop
whichever processes are running, copy their data to one contiguous region
of memory, change their segment register values to point to the
new physical locations, and thus have a large free extent of memory with
which to work. By doing so, the OS enables the new allocation request
to succeed. However, compaction is expensive, as copying segments is
memory-intensive and generally uses a fair amount of processor time.

There's tonnes of other algorithms such as best-fit, worst-fir, firstfit etc, but external fragmentation will still exist, and a good algorithm will only minimize it.





