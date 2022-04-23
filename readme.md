
Event Loop In Node:

# Explain in brief
=> Node.js is a single-threaded event-driven platform that is capable of running non-blocking, asynchronously programming. These functionalities of Node.js make it memory efficient. The event loop allows Node.js to perform non-blocking I/O operations despite the fact that JavaScript is single-threaded. It is done by assigning operations to the operating system whenever and wherever possible.

# how event loops works
=> The Event Loop takes the timer with the shortest wait time and compares it with the Event Loop's current time. If the wait time has elapsed, then the timer's callback is queued to be called once the call stack is empty. Node. js has different types of timers: setTimeout() and setInterval() .
```js
timers–>pending callbacks–>idle,prepare–>connections(poll,data,etc)–>check–>close callbacks
```
**OR** 

Working of the Event loop: When Node.js starts, it initializes the event loop, processes the provided input script which may make async API calls, schedule timers, then begins processing the event loop. In the previous example, the initial input script consisted of console.log() statements and a setTimeout() function which schedules a timer.

When the thread pool completes a task, a callback function is called which handles the error(if any) or does some other operation. This callback function is sent to the event queue. When the call stack is empty, the event goes through the event queue and sends the callback to the call stack.

The following diagram is a proper representation of the event loop in a Node.js server:
![nodejs2](https://user-images.githubusercontent.com/80479635/164878631-af7eabbc-1d24-4b8b-9a88-8550c48f5087.png)

# explain the phase in each cycle
=> Phases of the Event loop: The following diagram shows a simplified overview of the event loop order of operations:
![phasesofloop-300x240](https://user-images.githubusercontent.com/80479635/164878637-b2d6105e-aad7-4051-a556-4ec3689c6eb2.png)

 - Timers: Callbacks scheduled by setTimeout() or setInterval() are executed in this phase.
 - Pending Callbacks: I/O callbacks deferred to the next loop iteration are executed here.
 - Idle, Prepare: Used internally only.
 - Poll: Retrieves new I/O events.
 - Check: It invokes setIntermediate() callbacks.
 - Close Callbacks: It handles some close callbacks. Eg: socket.on(‘close’, …)

# explain how the event loop is making a non blocking system
=> Non-Blocking: Non-Blocking nature of node. js simply means that node. js proceeds with the execution of the the program instead of waiting for long I/O operations or HTTP requests. i.e the non-JavaScript related code is processed in the background by different threads or by the browser which gets executed when node.js completes execution of the main program and when the data required is successfully fetched. This way, the long time taking operations do not block the execution of the remaining part of the program. Hence the name Non-Blocking.

# explain how the web server is working and how node is helping it achieve scale
=> A web server is computer software and underlying hardware that accepts requests via HTTP (the network protocol created to distribute web content) or its secure variant HTTPS. A user agent, commonly a web browser or web crawler, initiates communication by making a request for a web page or other resource using HTTP, and the server responds with the content of that resource or an error message. A web server can also accept and store resources sent from the user agent if configured to do so.

Node is completely event-driven. Basically the server consists of one thread processing one event after another. A new request coming in is one kind of event. The server starts processing it and when there is a blocking IO operation, it does not wait until it completes and instead registers a callback function.

# Explain what libuv is
=> When using Node.js, a special library module called libuv is used to perform async operations. This library is also used, together with the back logic of Node, to manage a special thread pool called the libuv thread pool.This thread pool is composed of four threads used to delegate operations that are too heavy for the event loop. I/O operations, Opening and closing connections, setTimeouts are the example of such operations.

# Explain what setImmediate is
=> The setImmediate function is used to execute a function right after the current event loop finishes. In simple terms, the function functionToExecute is called after all the statements in the script are executed. It is the same as calling the setTimeout function with zero delays.

# Explain what is process.nextTick is
=> nextTick() taking a callback function which is executed after completing the current iteration/tick of the event loop. Phases of Event Loop in Node JS from nodejs.org. In the above diagram, process. nextTick ( ) is not the part of any phase of event loop.

# Explain the difference between setImmediate and process.nextTick
=> Understanding process.nextTick() method: Whenever a new queue of operations is initialized we can think of it as a new tick. The process.nextTick() method adds the callback function to the start of the next event queue. It is to be noted that, at the start of the program process.nextTick() method is called for the first time before the event loop is processed.
Syntax:
```js
process.nextTick(callback);
```
Understanding setImmdeiate() method: Whenever we call setImmediate() method, it’s callback function is placed in the check phase of the next event queue. There is slight detail to be noted here that setImmediate() method is called in the poll phase and it’s callback functions are invoked in the check phase.
Syntax:
```js
setImmediate(callback);
```
