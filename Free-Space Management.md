1. Free list
This structure contains references to all of the free
chunks of space in the managed region of memory. Of course, this data
structure need not be a list per se, but just some kind of data structure to
track free space.

Suppose we have a free list that contains 2 segments of 10 bytes each. In this case, 3 possible things can happen:
1. a request for memory greater than 10 bytes will fail i.e it will return NULL.
2. a request for exactly 10 bytes could be easily satisfied, since either of the 2 chunks of memory could be returned.
3. what will happen to a request for smaller than 10 bytes?

From the 3rd case, suppose we have a request for a single byte of memory. An action called <b>splitting</b> will happen.
1. <b>Splitting:</b> Allocator finds a free chunk of memory that can satisfy the request and split it into two. The 1st chunk will be returned to the caller; the 2nd chunk will remain on the list.
2. <b>Coalescing:</b> Another mechanism used by allocators.
Sample scenario:
- Suppose we start out with a free list that has 3 nodes. At each node, there's 10 bytes each of memory available. The middle node is in use. The other 2 nodes (at the start and end of the free list) are free.
- Suppose the node in the middle gets freed. So, we now have 3 nodes with 10 bytes each, all of which can be used. Even though the entire heap is now free, it's seemingly divided into 3 chunks of 10 bytes each.
- If a user requests 20 bytes, a simple list traversal will not find such a chunk (even though collectively we have 30 bytes available), and will return NULL.

<b>How does coalescing solve this problem?</b>
- in cases like these, allocators coalesce free space when a chunk of memory is freed.
- The idea is simple: when returning a
free chunk in memory, look carefully at the addresses of the chunk you
are returning as well as the nearby chunks of free space; if the newlyfreed
space sits right next to one (or two, as in this example) existing free
chunks, merge them into a single larger free chunk.
- Thus, using this strategy, our final list basically combines all 3 nodes together into single node of 30 bytes.

<b>Strategies to manage free space:</b>
1. Best Fit
The best fit strategy is quite simple: first, search through the free list and
find chunks of free memory that are as big or bigger than the requested
size. Then, return the one that is the smallest in that group of candidates;
this is the so called best-fit chunk (it could be called smallest fit too).
2. Worst Fit
The worst fit approach is the opposite of best fit; find the largest chunk
and return the requested amount; keep the remaining (large) chunk on
the free list. Worst fit tries to thus leave big chunks free instead of lots of small chunks that can arise from a best-fit approach.
3. First Fit
The first fit method simply finds the first block that is big enough and
returns the requested amount to the user. As before, the remaining free
space is kept free for subsequent requests.