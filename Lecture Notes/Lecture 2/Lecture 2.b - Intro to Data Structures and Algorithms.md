These notes are a crash course on common data structures and common algorithms. Most undergrad CS students take at least one course on this topic. We will be covering it very lightly, to get your feet wet and to help you synthesize what you know so far into more abstract problem solving techniques. Consider these the cliff notes for a real textbook on these topics rather than as a true substitute. 

But there is also a very important thing I want to share: **as much as people try to tell you so, memorizing data structures and algorithms problems does not make you a good developer/engineer/programmer.** I don't have the formal CS undergraduate education myself, but I have done those types of problems on sites like LeetCode, HackerRank or whatever, and I have done them timed under pressure while being forced to come to university on a Saturday morning. Here is my take:

Being able to solve problems under pressure is a valuable skill for everyone. Memorizing "silver bullet" solutions to memorized problems is for *people with nothing better to do.* Yes, the Google interview process basically expects you to do this (the same is true for many of the biggest software companies). But in my experience as a professional software developer, and in the opinion of many other actual software developers, this sort of filter does not get the "elite & highly skilled". All it gets are people who got a *formal education in CS very recently*  before they were interviewed. It means **jack shit** otherwise. For what it's worth, many people in the industry acknowledge the problems with this idea. 

At the same time, having a strong grasp on algorithmic principles can, and does, provide a lot of value (that's how Google was actually started, mind you). Note the difference here: learning algorithmic concepts and tools, giving you the vocabulary you need to find information, versus memorizing specific solutions so you can write them fast on a whiteboard for some jackass who can't program himself.

We have already used and talked about data structures and algorithms separately in this course (mostly during Lecture 1). Any direct solution to a problem that you described in steps was an algorithm. Any way you structured data was technically a data structure.

What we will do now is to quantify and analyze data structures and their behavior under certain conditions. During the exercises, you probably touched on some algorithmic problem solving but have not learned to generalize techniques or to compare two solutions in-depth. 

I will **not** be asking extensive detail-oriented memory questions and asking you to re-implement these. It's just a waste of your time. But what you **should** be able to do is to take a (sufficient) description of a structure or algorithm and implement it on your own -- it doesn't have to use every ace-in-the-hole, trick-up-your-sleeve, silver-bullet thing that is out there, just a naive implementation is totally fine.

These two topics are intimately intertwined, because the way we structure data affects how we use it in algorithms. I apologize, because this will be another long section. Don't worry, though -- the rest of Lecture 2 is much more conceptual.

# Complexity

When we refer to the *complexity* of an *operation* in computing, we mean something different than when we talk about the complexity of code. "Complex code" is hard to understand and refers to a property of the human-readability of the textual source code. *Complexity* as defined for operations refers to their characteristics as they are executed on a device.
All operations have an *associated time and space complexity*. 

*Time complexity* is the amount of processing time that operation will take. If we are processing *N* items, and we know each item takes 5 seconds to process, we can infer that the overall processing will take N * 5 seconds.

*Space complexity* is the amount of space an operation will take in memory (generally not counting the space taken up by the original arguments to that operation). If my solution to find the best item in a list of *N* items requires me to allocate an additional list of size *N*, and each element of the list takes exactly 4 bytes to store, then my solution will require an additional N * 4 bytes. 

Data structures can also have an associated space complexity. For example, if I want to store a 2-dimensional cross-reference matrix of all entered items (say, type weaknesses and strengths in Pokemon when attacking and defending), and N is the number of types I am storing, and each relationship takes 2 bytes to store the multiplier value, my data structure may actually take  2 * (N^2) bytes. 

Of course, we don't actually know *for sure* how much time or memory we will take, right? We have no idea what other factors about the device will affect the required processing time -- maybe the processor will be throttled, or maybe the operating system requires an additional byte of space for every allocation? Hence, we describe time and space complexity in what is called "Big O" notation, something like "O(N)". In the Big O notation, we are saying that the operation will take an amount of time *proportional* to the argument to O. By doing so, we can ignore these sorts of extraneous factors, and instead describe how our operation scales with the value or size of its arguments. 

Something that runs in O(1) time runs is said to run in *constant time* -- the amount of time may vary per platform or even per run due to factors out of our control, but the amount of time will be the same regardless of what input it takes. Likewise, an operation which runs in O(N), or *linear time*, takes an amount of time which is directly proportional to whatever N is. N could be the size of a collection that the operation is working on, or it could be a value (say, if I run a loop N times where N is an integer parameter). If I need to iterate over every possible combination of two values in a collection, my algorithm might run in O(N^2) (*quadratic time*, a special case of *polynomial time*). We can make similar statements for space complexity.

We also sometimes see complexities like O(logN), which indicates that the complexity basically "scales with the scale of N". This is a little confusing, but it means that it is the *magnitude* of N which matters, not N itself. Since scaling up N by some factor k takes increasingly *more* (k times as much) N *for every time you do it*, O(logN) grows slower than O(N). There are also complexities like O(k^N) and O(N!) which are both seriously explosive complexity. As an exercise, think about or research to see if there are any (typical) tasks which scale like this!

There are equivalent notations to Big O for saying that an operation will take *at least* a certain amount of time/space, or *at most* a certain amount of time/space, but eh, not important for the time being.

*Asymptotic complexity* refers to the complexity of an operation as *N* approaches extremely large values. You can think of this as the *limit of the complexity as N approaches infinity*. In this scheme, whichever term grows fastest with respect to N will dominate the actual value of the time taken: If an operation takes *N* seconds, plus an additional constant 5 seconds, then for large values of *N*, the 5 seconds will become unimportant in comparison. Likewise, if an operation takes *N*^2 seconds, plus *N* seconds, at large values of *N*, *N*^2 will make the *N* term insignificant. So we can essentially remove those slower-growing terms from our complexity, as they don't mean much. This type of complexity analysis is especially important for code which needs to accept extremely large sets of data.

*Amortized complexity* refers to a special type of complexity which reflects the operation's behavior *on the whole*, i.e. over a potentially infinitely long lifetime. Let's say you have some data structure where, the first time you need to get a value from it, you need to do some long and complex operation (like, I don't know, downloading all the data from a database, which takes 5 minutes). However, any access after this can be done super quickly (1 second). The *worst-case* complexity in this case is the higher value -- it's possible (but unlikely) that someone would use our data structure only once before discarding it, in which case they would have done the whole download process and taken 5 minutes, 1 second in total. But the *amortized* complexity is the lower complexity associated with accessing the elements after the download has been done. Over a long lifetime and a high amount of accesses, the "average complexity" tends toward the 1 second access time. 

In Big O notation, some data structures have "worst-case" complexities of O(N) when they need to reorganize and reallocate some space (say, if I am storing a new element in a collection and it needs to resize itself), but have amortized complexities of O(1) when accessing an element in general. 



# Data Structures

Again, a *data structure* is a way to collect data. Often in programming we are interested in collections of data, or in structures which establish meaningful relationships between data points. A data structure allows us to create a structure with meaning. A data structure can exist in the abstract, basically as a Platonic ideal, or it can exist as an implementation. 

Data structures define not just how data is arranged, but also what basic operations exist for that structure. For example, in a *stack*, we have two basic operations for interacting with it: "Push" (i.e. add an element on top) and "Pop" (i.e. take the element off the top).  In the abstract, we can say that a stack only has these two operations. (In reality, we could *probably* find a way to move something out of the bottom of a stack, like by kicking out the bottom element from underneath -- but we are exploiting properties of a *real* stack rather than an *abstract* stack).

For the stack example, Push and Pop form the interface for our object. External users of the stack data structure use only those two operations to interact with it, and our stack gives some contractual guarantees about the behavior its users can expect. Using those operations, we can compose more complex algorithms which solve a problem. 

It's important to remember that you are not necessarily *trying* to shoehorn these structures in in any form -- they offer nothing on their own. But if there is a good implementation which works for your task, use it! Why code it yourself unless you *really* need custom functionality? When you need that, you'll know.

## Common Data Structures
For each of the common data structures, we will describe them and briefly go over complexity of their operations. At each stage, I strongly encourage you to think about how these are actually working -- or how you  might implement them yourself. Look up sample code if you want. Don't just take my statements about complexity as fact and memorize it.

### Array
We've seen the Array before. An Array is usually defined as a fixed-size block of continuous memory, containing multiple elements, which allows "random" (i.e. out-of-order) access. Does this ring any bells? "Random Access Memory"? Yeah, essentially our entire system memory can be treated like this.

The notion that it is of fixed size means that "resizing" an Array really means allocating a new array and moving elements over.

An Array's space complexity is generally O(N) where N is the amount of (possible) elements. In many cases, a space with value `NULL` is taken to indicate the end of the usable content of the array. 

Writing to or accessing an array is of time complexity O(1) -- we can directly access its members in constant time, provided we have an index. The complexity of *inserting* an element somewhere in the array besides the end is O(N) -- we need to shift all elements in the array up by one space, or we otherwise need to individually copy all the elements including the new one to a newly created array.

It's worth noting that a Vector is just an Array with some wrapping operations allowing it to be resized.

It's also worth noting that Arrays are *blazingly* fast when used correctly. All elements being contiguous in memory means that the CPU's cache will *very often* have the appropriate data loaded.... sort of like having a defragmented hard disk drive. "Smarter" storage which exists fragmented in memory often gives up this cache-locality benefit.

### List (Singly Linked, Doubly Linked)
A List is usually defined as a structure where we have nodes/entries which contain both a value and a pointer to the next item in the list ("Linked List"). This means that this structure is not necessarily contiguous in memory. We can also have doubly-linked lists, where entries contain a pointer to both the next and previous elements. Since we can set these pointers arbitrarily, circular lists are also possible. Rearranging the list, or excising chunks of it, can be done just by changing where the "next" pointer points to on one or more nodes/entries (and the "previous" pointer too, if doubly-linked).

The "list" itself usually is either an object which points at the head (and/or tail), or is just a single node object which is the head. In the latter case, the head of a list will usually not point to a "previous" node unless the list is circular. 

A List's space complexity is O(N), where N is the number of entries.

Accessing a specific element of a List requires us to traverse each previous element from some starting point, and so has time complexity O(N). Iterating over the list in general needs the same.

Inserting or removing an element at the beginning or end of a linked list is O(1), because it is a single set of pointer-swaps. However, inserting or removing an element somewhere in the middle is O(N) because we first need to *get there* to do the swap. Of course, if you already have a handle to that point in the list, the rest of the work is still O(1).

As noted in the Array section, Lists like this can cause "cache misses" which slow performance in practice.

### Queue - First In First Out
A Queue is pretty much exactly what it sounds like. You *enqueue* an element to insert it to the beginning of the queue, and *dequeue* to take an element off the end. That is: First In, First Out. Usually, the queue exists in a contiguous memory space, which means it does have a pre-determined limit. However, within that limit, there are tricks to never have to re-arrange the elements no matter how much the beginning and end shift around within the limited space. 

Think for a bit: **How might you implement that?** (Hint: what does the "modulo" operator (%) mean?)
You don't need a full implementation here, just the idea.

Queues also have space complexity O(N), where N is the number of elements. 

Searching through the queue for a specific element is O(N), whereas inserting and deleting elements is O(1).

Worth noting, there are more specialized (and more interesting) forms of queue, like priority queues, which serve higher priority elements first. There's also double-ended queues, also called *deque*s.

### Stack - Last In First Out
We've talked about this one! Again, you *push* elements to the top, and *pop* them off the top in the same order, like a stack of plates. And like a Queue, the stack exists within a fixed amount of space. (We'll see that for ourselves later in these notes >:^) )

Space complexity is O(N).
Searching for a specific element is O(N). Inserting and deleting is O(1).

But maybe you don't really see how this relates to problems. If that's the case, I have an exercise just for you! Create a function which tells me if a string is a palindrome.

### Graph
Graphs are basically networks of elements (nodes or sometimes "vertices"), with links ("edges") in between. Those edges, themselves, can have associated values (like "length"), or can be either bidirectional between elements or unidirectional. Graphs see a huge amount of use in many advanced applications. Google Maps would be one -- how do you think that is?

Like other structures, Graphs have a hell of a lot of specializations and subspecializations for specific applications. For instance, directed acyclic graphs are guaranteed not to have any circular paths between nodes. What graphs do you see in your daily life?

Complexity of graphs can be complicated and depends upon implementation... see [here](https://en.wikipedia.org/wiki/Graph_(abstract_data_type)#Common_data_structures_for_graph_representation) for details. 

### HashMap/HashTable (also called *Associative Arrays*, Dictionaries)

This is a fun one. Hash tables, also called hash maps (and associative arrays, and dictionaries....) essentially map "keys" to "values". Passing in some data as the "key" generates a corresponding value. Underneath the surface, they use a "hashing function" to turn the key object into an index, and use that to index into memory to access the value.

Space complexity for a hash map is O(N) where N is the number of possible elements. But note that we need the hash function to make an index which is within bounds. As a result, the actual space used is usually significantly higher, to reduce the possibility of *hash collision* where two objects being hashed access the same location in memory. You generally don't have to worry about this accidentally happening -- most implementations do some double-checking to help prevent this.

Time complexity here is also interesting. Searching, insertion and deletion are all O(N) in the worst case (where the hash table needs to be remade), but are have *amortized* time complexity O(1). The reasons why are fairly deep in the details.

Hash tables are a super deep topic if you want to get into it, but if you don't, I don't blame you.

### Trees (many kinds)
You can imagine what a tree structure "looks like". In this structure, there are nodes. Each node can have one or more children, but each node can have only one parent. The root node is a special case node with no parent. These constraints guarantee that there are no cycles in a tree.

The amount of special subtypes of tree actually makes it very difficult to discuss properly here! Know that *recursion* is often useful for traversing trees. We will learn what that means soon.

There are two ways to search through trees: "depth-first" and "breadth-first". Depth-first search involves going down one side completely first, breadth-first search involves scanning over all children at successive layers of depth.

Wikipedia gives us the following list of common tree operations:
- Enumerating all the items
- Enumerating a section of a tree
- Searching for an item
- Adding a new item at a certain position on the tree
- Deleting an item
- [Pruning](https://en.wikipedia.org/wiki/Pruning_(algorithm) "Pruning (algorithm)"): Removing a whole section of a tree
- [Grafting](https://en.wikipedia.org/w/index.php?title=Grafting_(algorithm)&action=edit&redlink=1 "Grafting (algorithm) (page does not exist)"): Adding a whole section to a tree
- Finding the root for any node
- Finding the [lowest common ancestor](https://en.wikipedia.org/wiki/Lowest_common_ancestor "Lowest common ancestor") of two nodes

This is one of those cases where, when you know your objects are related in a treelike structure, you do that, then later Google the solution for doing the above for a certain type of tree.

A Heap is basically a tree, subject to some rules about the ordering of elements. This sort of rule-based structure can often make searching much easier.

### ... and More!
But we won't be covering those unless we need to for a specific problem. Phew.

### The role of Data Structures
Algorithmic users of a data structure are not usually concerned with *how* the structure does its job, or what internal mechanisms it uses, only that it fulfills a certain role for their solution. 

The exception is when one form of data structure offers a performance advantage over another. To understand this, we will explore *complexity* of data structures and their operations. 

# Algorithms

Many common tasks in software engineering are solvable, at least in part, by some combination of filtering, sorting and searching for individual elements in collections of data. We will look at some ways to use data structures to perform these tasks and write flexible algorithms which can work for many types of data.

## Recursion vs Iteration
We have already done iteration in this course. Any time you run a loop, you are iterating. In the context of a list of items A, B, C... we might say that iteration 1 handles item A, iteration 2 handles item B, iteration 3 handles item C, and so on. Even if we have a 2 (or higher) dimensional structure, we can usually iterate over those elements one at a time in sequence. Even if we take multiple elements at a time, and/or skip over some, it's still iteration. In this sense, iteration is a type of control flow where we go in circles before eventually returning to normal linear control flow.

*Recursion* is a different type of control flow, one which is often confusing to beginners.
Recursion can be defined as follows:
	Recursion can be defined as follows:
		Recursion can be defined as follows:
			Recursion can be defined as follows:
				...

OK, I'm joking, but this helps illustrate what recursion is. A recursive process is a process which occurs in a self-propagating way. In programming, *recursion* refers to when a segment of code calls itself. 

You may be surprised to learn that this compiles:
``` cpp
int getRecursiveResult() {
	int result = getRecursiveResult();
	return result;
}

int a = getRecursiveResult();
```
Ah, but it does compile! *But does it run* (properly)?

First, give it a try in your head -- what will happen? Then try to compile and run it before continuing.

If you ran it, you will probably see a specific type of runtime error, a *stack overflow*. Given what you were told earlier about function calls and "the stack", can you explain this?

A stack overflow occurs when the amount of memory allocated for the stack (which is OS-controlled) is overrun. Because each function call takes up space on the stack, it is possible to reach a level of function call depth which runs over the allowed bounds. Normal functions will find it *exceedingly difficult* to reach this point. Recursive functions have no problem doing so, because they call themselves. If there is no *termination condition* which is reached, the recursive function calls will inevitably fill up the stack.

We might imagine adding such a condition (and a parameter to help facilitate it):
```cpp 
int getRecursiveResult(int counter) {
	if (counter == 0) {
	 return counter;
	} else {
	 return getRecursiveResult(counter - 1);
	}
}
```
What will this eventually return? Try to draw the stack structure if we initially call `getRecursiveResult(4)`.

OK, but what is this useful for? This is very often useful when a data structure itself is recursive. For example, in a tree-like data structure, each node of the tree is either a leaf (has no "child nodes") or is itself a tree (has child nodes of its own, which may also have children...). If each node also has an integer value, then the sum value of the entire tree is the sum of its subtrees, and the sum of each of those subtrees is the sum of each of *their* subtrees...

If each node has a *left* node pointer and a *right* node pointer (which may be `nullptr` to indicate they don't exist):
``` cpp 
struct Node {
	int value = 0;
	Node* left = nullptr;
	Node* right = nullptr;
}
Node* myNewTree = new Node();
myNewTree->value = 5;
// ... skip the steps where we insert all the tree elements...

int getSumOfTree(Node* root) {
	if (root == nullptr) {
		return 0;
	}
	return root->value + getSumOfTree(root->left) + getSumOfTree(root->right); 
}

getSumOfTree(myNewTree);
```

Recursion is often not the most efficient way to solve a problem, but sometimes it is actually the most straightforward.

How would you describe the complexity of our `getRecursiveResult` above? Try this now as an exercise. What about `getSumOfTree` we just wrote?

## Sorting

Sorting is emphasized so much in algorithms for a pretty simple reason: sorting is a nice precursor when solving other problems. If you can sort a structure, you have (generally) not changed the meaning of the structure itself or its individual elements, just made it more convenient for certain operations... And if the order *does* matter, then you probably don't need to sort anyway. Hence, if you have the opportunity to pre-sort a structure, it's good to do so first. 

Naturally, *which criteria to sort by* is the hard part. Often, when using functions made for sorting, we can pass in a "key function" which is used as the thing to sort by. For instance, sorting numbers, it's fairly clear we want to sort by their value... at least usually. But what does it mean to sort `Color`s or `Car`s? You have to write that for yourself as a function which compares two `Car`s and chooses which is greater. There is also a more general form of this you can produce which is called an "ordering type" but that's a bit complex to explain here.

A common way of sorting elements is to put them into a different kind of data structure, then apply operations to that data structure in order to get the final order. For example, you might put all elements into a tree, then sort the tree using a couple of simpler operations, then return the elements back into another structure in some defined way.

The various sorting algorithms out there have strengths and weaknesses depending on assumptions you can make about your data. For example, "bubble sort" is a method that relies on successively "bubbling up" values to the end of the list. Hence, the worst-case scenario is that the list is in fully-reversed order, in which case it has O(N^2) time complexity. But if your list is already mostly sorted, it is only O(N) time complexity. 

I recommend checking out the following [list]([https://dev.to/koladev/8-must-know-sorting-algorithms-5ja](https://dev.to/koladev/8-must-know-sorting-algorithms-5ja "https://dev.to/koladev/8-must-know-sorting-algorithms-5ja")) for a cursory overview. Check out that list and look at their explanations, they're fairly nice. You might also look up other resources for a more in-depth explanation, but this list also has Python code samples which might help.

On that note, C++ and Python both offer built-in sorting functions in `std::sort` and `sorted()`. Seriously, just use those unless you really have some reason to need something else.

### Searching

Searching for the element at a specific index is one thing. Searching for the existence of a certain value, or a value meeting certain criteria, in a collection is a different thing entirely. In a general structure, you may be doomed to trying the criteria on every element, but in a more specialized structure, the rules of organization or sorting may mean that you can infer more information and therefore skip some steps.

For example, if all child nodes of a node on a heap must be less than their parent, and that parent has a lower value than the one you are looking for, you know that your desired element does not exist anywhere further down that subtree.

However, sometimes the thing you are searching for is of a different meaning than the elements themselves. For example, Dijkstra's algorithm and A*, which are both used for path-finding on graphs, don't return a single element or a value representing whether it is in the set and where. Rather, they return a path, which is an ordered set of nodes to visit to take the shortest path. This path could be represented as *another* (very simple) graph, or could be considered its own type of object.

The logic in these cases is almost impossible to generalize since it's so specific to the meaning of the items and the structure they are held in. In the above cases, nodes are actual *locations* and edges between them are *paths*. But other graphs treat nodes as, say, atoms, and edges as bond strengths.

I would like you to research one algorithm of interest to you. It could be PageRank which powers Google (at least, the original implementation, which is public knowledge), it could be the A* algorithm usually used in video game pathfinding, it could be a cryptographic algorithm... whatever you like, just find one. Try to understand it in (reasonable) depth. I'm not asking you to mathematically prove the algorithm again or anything. Just provide a brief summary of the way the algorithm accomplishes its goal, *in your own words*. Diagrams get bonus points.

# Conclusion
As I said, this is a cursory review. However, you should now be reasonably well equipped (especially once you do the corresponding assignments) to think independently about data structures and algorithmic approaches. For those of you who will be handling large amounts of data in your project, you will find this immediately useful. 

For others, it will help you critically think about your solutions. After all, maybe that function I'm running on every frame of the game *doesn't* need to account for every object in the map at the same time... Remember that when we describe time complexity in big O, we are describing what the time/space needed is *proportional to.* That "N^2" might, in reality, have an "88" next to it as coefficient, or it might have a "2". That might not really matter in the abstract, but it does in the real world. 

Of course, some of that coefficient is basically baked into the hardware. But you'd be surprised just how much you can optimize something if you really feel like it. Video games are sort of a classic arena for creative forms of optimization like LODs, enemies moving at lower framerates or even being culled entirely when they're too far behind you, and so on. Similarly, if you have an expensive function which "needs to run for every object every frame to check for a special condition"... does it? Could you split the objects into 4 chunks and run those in successive frames, then find a result on the last one? Could you refactor it so that *an event close to what you are interested in* calls the function which does some deeper checking? The possibilities, again, are endless.

Anyway, that's the crash course on data structures and algorithms. It's easier from here.


