---
layout: post
title: The 4 differences between observables and Promises in Javascript
date: 2020-08-26
excerpt: The difference between promises and observable is one is my favorite interview questions. This question won't reveal their technical competence but might show some assumptions they have about JS and tools. So what are the differences?
tags: [javascript, observable, rxjs, promises, async, await]
published: true
---
Javascript is a single-threaded language and it is blocking by nature. Still, we can do things asynchronously without blocking further code execution. Speaking of async, there are multiple ways to run async operations. I mostly use Observables and Promises. There is async/await, but it is just syntactic sugar over promises.

The difference between promises and observable is one is my favorite interview questions. This question won't reveal their technical competence but might show some assumptions they have about JS and tools. So what are the differences?

### Frequency

Observables are a stream of data or events, they are capable of firing more than one value over time. The below code will fire up every second, indefinitely.

```js
import { Observable } from 'rxjs';

const greeting$ = new Observable(observer => {
    const clear = setInterval(() => {
        observer.next('Hello, there');
    }, 1000);
    // Need to handle the interval here, otherwise you'll end up in a infinitely firing observable.
});

greeting$.subscribe(res => {
    console.log(res);
})
```

Promises are a single event. Once resolved, the promise is done. They are best suited for one-time events such as results of an HTTP call.

```js
const greet = new Promise((resolve, reject) => {
   resolve('Hello, there');
});
greet.then(success => {
    console.log(success);
});
```

> Ironically, the Angular team decided to use observables for their HTTP library HTTPClient, for good reasons. This is a great architectural decision that is further explained by the other reasons in this post. In short, observables give better control on execution and manipulating results than promises.

### Cancellation

Observables can be canceled, there are <a href="https://blog.bitsrc.io/6-ways-to-unsubscribe-from-observables-in-angular-ab912819a78f" target="_blank" rel="noopener noreferrer">multiple ways</a> to do it. By canceling, any ongoing calls and further emissions from the observable will be ignored. In reality, observable can wrap a lot of things such as user events, HTTP requests, and promises. Not all of them can be canceled or reversed once you made the call. Regardless, further results will be ignored once you canceled the observable.

But promises can't be canceled once you called them. There
<a href="https://github.com/sindresorhus/p-cancelable" target="_blank" rel="noopener noreferrer">are</a>
<a href="https://github.com/hjylewis/trashable" target="_blank" rel="noopener noreferrer">some</a>
<a href="https://github.com/permettez-moi-de-construire/cancellable-promise" target="_blank" rel="noopener noreferrer">libraries</a> that support cancelable promises. But ES6 promises don't support cancellation. The callbacks of a promise will be handled by the browser's event loop. so you can do nothing once it is called.

### Asynchronous

Promises are always asynchronous, even if they are resolved immediately. To know why, we need to understand the event loop in Javascript.

### What is Event Loop?

_If you're already aware of what the event loop is, you can safely skip this section._

To put it simply, event loop is Javascript's solution to handle the blocking tasks. JS just moves the blocking things to a different thread.

But Javascript is single-threaded, right? You may ask.

Javascript is single-threaded for a developer who uses it. You can't fire up a new thread for something and then get results back as we can in Java or Rust. But it doesn't mean everything you write in javascript is running within a single thread.

when you execute the following code, something happens.

```js
function getTitle(gender) {
  // This is a vague simplification, gender is not binary.
	return (gender === 'M') ? 'Mr ' : 'Ms '; 
}
function print(content) {
 console.log(content);
}
function greet(name, gender) {
 const title = getTitle(gender);
 const message = `Hello, ${title}. ${name}`;
 print(message);
}

greet("Draper", "Male");
```

Each function you call will be added to a stack, that is called the call stack. As each function returns, that function is popped out of the stack and the previous function is resumed. Once all the functions have been returned the stack will become empty.

When you make a lot of function calls like a recursive call that went rogue, you'll get the "Maximum call stack exceeded" error. It means, there is only so much the call stack can hold and your code exceeded that limit.

Let's imagine what will happen without the event loop. In the call stack, When a function is running, the other functions have to wait. When waiting for some functions such as network calls, the UI will freeze. If there is no event loop, all other code has to wait for the network call to finish and comeback. To overcome this, JS pushes the tasks that may take a long time into a separate thread.

```js
setTimeout(function (
	console.log("I'll show up after 5 seconds");
) {}, 5000);
```

In the above code, if JS has to put that into the stack and wait 5 seconds, all other functions and user interactions will suffer. So it pushes async tasks to a different thread and continues with the rest of the synchronous code.

Once the asynchronous function finished running, JS will push the callback function to a queue called callback queue. Technically, there are two queues Microqueue and Macroqueue. For the sake of simplicity, let's assume there is only one queue. Once a function got pushed into the callback queue, the event loop will kick in. The event loop will check if the current call stack is empty. The callbacks will not be executed until the current call stack is empty.

If the current call stack is not empty it means something is in-progress. Once the call stack becomes empty the event loop will push the items from the callback queue into the call stack. If there are multiple items in the queue, the items will be pushed into the stack one after the other and will be executed. If you need a deeper explanation of the event loop, check out this
 <a href="https://www.youtube.com/watch?v=8aGhZQkoFbQ" target="_blank" rel="noopener noreferrer">great talk</a>

### Back to promises

Javascript promises are using the microtask queue in event loops. When you create a promise, it's callback goes to the callback queue even if you resolve it immediately. It has to wait until the stack is empty this is the reason promises are always async. The process of items added to the call stack, executed, and the callstack becoming empty again is the event loop. Promises always need one more iteration in the event loop to resolve.

Observables can be both synchronous and asynchronous, depending on the function the observable is executing. For ex, If an observable wraps a promise, it will be asynchronous. I was reading into the rxjs source code to understand how observables are working and found 
<a href="https://github.com/ReactiveX/rxjs/blob/78032157f5c1655436829017bbda787565b48c30/src/internal/Observable.ts#L116" target="_blank" rel="noopener noreferrer">this</a>

> Remember, callbacks provided to `subscribe` are not guaranteed to be called asynchronously. It is an Observable itself that decides when these functions will be called. Always check the documentation for how given Observable will behave when subscribed and if its default behavior can be modified with a `scheduler`.

The best thing about observables is You don't really have to worry about if an observable is synchronous or asynchronous. It just works the same way for the observer. It makes writing code a lot easier. You're just going to subscribe, and the internals are well abstracted.

### Evaluation

Promises are eagerly evaluated. When you define a promise and give a callback function to its constructor, that is called immediately. This is useful for the things you want to fire immediately.

Observables are lazily evaluated. When you give a callback to the constructor of an observable, it'll be only executed after subscribe has been called on that observable. I found the following
 <a href="https://github.com/ReactiveX/rxjs/blob/78032157f5c1655436829017bbda787565b48c30/src/internal/Observable.ts#L86" target="_blank" rel="noopener noreferrer">comment</a> from observable's source code that explains this fact in detail. This gives as much more control when using an observable.

You can pass the reference to an observable, and it won't be executed until someone subscribe to it. It also allows us to transform that observable's stream in multiple stages. I ofter create one observable at the service level and then add more operators that transform the data into different format in different places as required.
