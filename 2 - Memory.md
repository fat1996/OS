While a program runs, there's 2 kinds of memories at work:
1. Stack
- the allocations and deallocations of the stack are <b>implicitly</b> managed by the compiler for us.
- this is why it's called <b>automatic</b> memory.

For e.g
void func(){
	int x; //declares an int on teh stack.	
}
- the compiler does all the work for us here i.e creates space on the stack for the declared var, and once we return from the function, the compiler deallocates the memory for us.

2. Heap
- we need the heap for long-lived memory.
- all allocations and deallocations need to be explicitly managed by us.

For e.g
void func(){
	int *x = (int *) malloc(sizeof(int)); //declares an int on teh stack.	
}
Some notes about this code snippet:
1. both stack and heap allocation happens here.
2. stack because the compiler knows to make room for a pointer to an int.
3. heap because when malloc() gets called, it requests space for an int on the heap.

<b>The malloc() call:</b>
- we pass it a size asking for some room on the heap
- if this succeeds, it gives us back a pointer to the newly allocated space.
- it it fails, returns NULL.

<b>Common errors with memory management:</b>
1. forgetting to allocate memory
- many routines need memory to be allocated before they're called.
For e.g, strcpy(dst, src) copies a string from a source pointer to a destination pointer.

Like so:
char *src = "hello";
char *dst;  //no memory allocated here for dst pointer.
strcpy(dst, src);

- we'll get a seg fault when the above code will be run.

Corrected code:
char *src = "hello";
char *dst = (char *) malloc(strlen(src) + 1);  //no memory allocated here for dst pointer.
strcpy(dst, src);

2. not allocating enough memory
- called a buffer overflow

For e.g,
char *src = "hello";
char *dst = (char *) malloc(strlen(src));  //too small
strcpy(dst, src);

3. forgetting to free memory
- happens when we forget to free memory.
4. freeing memory before we're done with it
- mistake is called a <b>dangling pointer</b>
5. freeing memory repeatedly
- happens when we free memory more than once.
- known as double free.
- result of doing so is undefined.

- malloc() and free() are <b>library calls</b>, not <b>system calls</b>. This means that the malloc library manages space within our virtual address space, but itself is built on top of some system calls which call into the OS to ask for some more memory or release some back to the system.