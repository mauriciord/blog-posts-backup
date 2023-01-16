# JS - The Event Loop

It's been a long time without posting anything here, but I'm back.  
Let's talk about Javascript's Event Loop. As you may know, JS is a ***Single-Threaded*** language, so it can only execute 1 task simultaneously.  
Assuming we have the *Javascript Engine*, a program built to execute Javascript code, we need to consider that each browser has a *Javascript Engine:*

* **Chrome** and many other Chromium browsers have the V8 Engine
    
* **Safari** has the JavascriptCore
    
* **Firefox** has the SpiderMonkey
    

These engines can be different in implementation across browsers, but at least they contain two parts:

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1672928493142/15200540-0ec7-4965-ab13-7977127110c8.png align="center")

* **Memory Heap:** A data store with memory allocation to store objects
    
* **Call Stack:** It's a stack that keeps track of running functions. Every time the code is calling a function, it pushes that piece of code (*stack frame*) onto the stack. The *stack frame* will contain information about the functions and their local variables.
    

> When a function is called, it goes onto the top of the call stack, and when that same function ends, it's popped out the call stack. **LIFO** (last-in-first-out) is the name given to this principle.

OK, now you've got the basics of the Call Stack. But, what would happen in the case of incrementing a `setTimeout(foo, 2000)` in the code?

## JavaScript Runtime Environment

The JS Engine runs our code in this larger environment known as *JavaScript Runtime Environment.* It provides some **web APIs** to be able to access the browser's functions like DOM manipulation, timers, HTTP Requests, and so on.

> But, what would happen in the case of incrementing a `setTimeout(foo, 2000)` in the code?

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1672943518395/bf31aa60-c576-4936-9563-6d11821db72a.png align="center")

### Task Queue

**Web APIs** move callbacks into the **Task Queue**, to wait for the **Call Stack** to be empty, and start executing that callback function. Then, the **Task Queue** is a data structure responsible for storing asynchronous callbacks to be moved to the **Call Stack**.  
*On the example above, you can notice that after the delay of 2000ms, the foo() function has been moved to the Task Queue, waiting to be executed when the Call Stack becomes empty.*

> This queue is also known as the "Macrotask Queue", "Callback Queue", or "Message Queue".

This overall process is known as **Event Loop**:

1. Remove one task from the **Task Queue**
    
2. Executing code until the **Call Stack** is empty
    
3. Rendering any changes to the DOM
    
4. Going to *step 1*
    

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1672950626161/a4ca15e2-1800-409e-8c1e-15df4d1e1ebc.png align="center")

%[https://codesandbox.io/embed/pedantic-tdd-upyc0q?expanddevtools=1&fontsize=14&hidenavigation=1&theme=dark&view=preview] 

## What about Promises?

After Promises have been introduced to the language, there's a new Queue in the picture, and that one is the **Microtask Queue**.

## Microtask Queue

Known as well as *Job Queue*, the **Microtask Queue** is responsible for holding the callback function of Promises.  
The known callback functions of Promises are `.then()`, `.catch()`, `.finally()`, and `async/await` .  
They can be manually queued using the `queueMicrotask()` function.  
**Microtasks have more priority compared to Macro tasks** because the entire microtask queue should be empty before Browser checks the Task Queue.

Then, with those new concepts in mind, we need to update our **Event Loop** workflow:

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1672955423238/20e961e3-559d-4905-9a51-e6d641b7d852.png align="center")

1. Removing one task from the **Task Queue**
    
2. Executing code until the **Call Stack** is empty
    
3. Running all microtasks until the microtask queue is empty
    
4. Rendering any changes to the DOM
    
5. Going to *step 1*
    

For all of these steps, we have a name to point to all of them, and we call as **Concurrency Model**.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1672956249099/a17a0b5c-88ff-4912-972d-e378f2ce3b97.png align="center")

## Conclusion

Big computations and slow functions can make the entire page unresponsive. When you fall into situations like these, you can try solutions like **chunking**, or even web workers.  
The main idea behind chunking is to split big functions into smaller functions. You can achieve that using `setTimeout` for some iterations, forcing them to be chunked, and moving them to the end of the **Task Queue**.  
I've got a friend that was used to saying: *"There's nothing a setTimeout() can't fix"*.  
  
I hope you've enjoyed this text. Please, press **like** and don't forget to share it with your colleagues or friends. See you!

### References

* [What the heck is the event loop anyway? – Philip Roberts (](https://2014.jsconf.eu/speakers/philip-roberts-what-the-heck-is-the-event-loop-anyway.html)[jsconf.eu](http://jsconf.eu)[)](https://2014.jsconf.eu/speakers/philip-roberts-what-the-heck-is-the-event-loop-anyway.html)
    
* [The event loop - JavaScript | MDN (](https://developer.mozilla.org/en-US/docs/Web/JavaScript/EventLoop)[mozilla.org](http://mozilla.org)[)](https://developer.mozilla.org/en-US/docs/Web/JavaScript/EventLoop)