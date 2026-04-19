Why redis is so fast even its single-threaded and also can handle multiple connections?

So fast:
- data is stored in-memory(Stores it in RAM) which is faster than disk.
- no locks, semaphores as its single threaded only, multithreaded systems need that not single threaded.
- It uses highly efficient data structures like hash tables and stuff, so many operations can be completed in O(1) time only.

How can handle multiple connections?

- actually it checks the connections which are ready, and process them one by one, and switch connection very very fast, so it feels like its handling all at once but not.
- by ready, what I mean is if any connections want some read/write, or like client has sent data and its waiting to be read, and ignore all other
connections which are not ready.
- Then again, after executing with these connections, again asking OS for the connections that are ready.
- Yes, redis asks the OS which all connections are ready.
	These are the OS mechanisms that tell Redis which connections are ready.
		epoll (Linux)
		kqueue (Mac)
		select
		
- So this whole phenomenon has few names:
	. Event-Driven Architecture: 
		System reacts to events like:
			“new data arrived”
			“socket is ready”
	
	. Event Loop
		A loop that keeps doing:
			ask OS → get ready connections → process them → repeat
	
	. Non-Blocking I/O
		Redis never waits for slow things
		If something isn’t ready → it moves on
	
	. I/O Multiplexing
		This means: One thread handling many connections using OS help.
		This is how event loops are implemented also.
		
		Without I/O multiplexing:
			Event loop would have to check every connection manually (slow)
		With it:
			OS does the hard work efficiently
			
	
	Event Loop uses I/O Multiplexing to know which connections to handle next.
	
	
	That's how they work together
	Event Loop starts
			↓
	Ask OS (I/O Multiplexing)
		  ↓
	OS returns: "These connections are ready"
		  ↓
	Event Loop processes them
		  ↓
	Loop repeats
