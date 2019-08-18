---
layout: post
title: "RxJs - You're probably doing it wrong"
date: 2019-02-25
excerpt: "How to take advantage of RxJs?"
tags: [angular, rxjs, RxJs, javascript, Observable, operators, subscription, streams]
published: true
featured: "/assets/img/posts/how-to-rxjs/rxjs-logo.png"
---
I have to agree that the above title is pretty strong and kinda click-a-bait. But trust me, I was using the library wrong, I was not utilizing it to its full extent and was writing code which should be far more easier. I am nowhere claiming to be an expert in RxJs but I have done these mistakes in the past and still witnessing others doing the same, so I took a chance to look back and wrote what I should have been told a year or two ago about RxJs.

<figure>
	<img src="/assets/img/posts/how-to-rxjs/rxjs-logo.png">
</figure>
To understand the problem behind this let's get to the background, if you're like me, you're probably forced into RxJs by Angular and you were just using it only to subscribe to http calls and not even bothered to look at its documentation.

I hope few of us tried to use RxJs and tried push our limits but failed or gave up, why RxJs seems so hard at the beginning? Like everything, RxJs indeed has a learning curve to it but it isn't the reason. Its our approach towards programming, most of us working in either procedural or OOP style and RxJs requires little different approach towards how you handle events, input and output.

 RxJs has two distinct characteristics,
 
 - It treats any event or input as a stream which can flow through various functions, operators.
 
 - It mostly uses pure functions to produce values and relies on them to keep the state error free.
 
 When I started with RxJs I had no clue on how or when to use those operators it has, because it was not very obvious. All those RxJs tutorials I've read mostly never used any practical examples. They'll create an ***observable.from('thin air')*** and manipulate it, but I was never be able to imagine where that operators will be helpful. I have tried my best in this post to stay relevant and to use real use cases as examples.
 
 <h3>
 <a target="_blank" href="https://www.learnrxjs.io/operators/transformation/map.html">Map operator </a>
 </h3>
 
 Map mainly useful for transforming data. What it does is that every time your source observable fires, it takes the value and send it to the function you defined and then returns the result of that function as an observable. Imagine the following code which is not very unusual in any angular project.
 
 {% gist 2f7a592cd6f741f7391d813298392c34 without-map.js %}
 
 The data returned from the API is all we care about, So instead of manipulating it inside the subscription we can use the map operator.
 
  {% gist 2f7a592cd6f741f7391d813298392c34 with-map.js %}
 
The map operator is meant to do all the transformation with the observable data and to return a new observable which the subscribe function will receive. But what if the API returns an empty collection? our map will fail. We will see how we can fix it.
 
 <h3>
 <a target="__blank" href="https://www.learnrxjs.io/operators/filtering/filter.html">Filter operator</a>
 </h3>

 Filter operator is similar to array filter, in fact all the operators are inspired by the Array functions in JS, so when your source observable emits a value, the filter takes the value and sends it to the predicate function you defined and if the function returns true it emits the observable.
 
 We use the same functions we used in our previous map example but by safely checking if their is any data returned or not. Normally we would use an if condition,
   {% gist 2f7a592cd6f741f7391d813298392c34 without-filter.js %}
   
   In the above code, we are acting on the data regardless of whether it satisfies our condition or not. We can rewrite this using the filter operator like below, which will only act upon the data if it satisfies the predicate function provided.
 
   {% gist 2f7a592cd6f741f7391d813298392c34 with-filter.js %}
   
The above subscription will only be reached when the API returned some data.
 
 <h3>
    <a target="__blank" href="https://www.learnrxjs.io/operators/filtering/take.html">take</a>
   </h3>
     
   Imagine if you have an source (observable) which emits multiple values but you only care about the first `n` values. For example if a user clicks on a button and you're making a call for that, if you directly bind your html event to the http call, chances are multiple subsequent http calls. But we can use take(1) to prevent this as it'll emit subsequent clicks after the first click.
   
  {% gist 2f7a592cd6f741f7391d813298392c34 take-example.js %}  
   
 You can see this into play in this <a href="https://stackblitz.com/edit/rxjs-take-example?file=index.ts" target="blank">stackBlitz</a>, try commenting the take operator in the click event and you'll see it fires http request each time you click the button.
 > Note: This is not the best way to block multiple requests from being fired, for example, you can't make the second request even after the first request has returned. 
 
 <h3>
 <a target="__blank" href="javascript:void(0);">forkJoin</a>
 </h3>
  Forkjoin is a combination of operators which can be used mostly to get rid of nested subscriptions. Imagine that you have two http requests which are not dependent on each other, but you need the results from both of them to perform some operation.
  
  Let's take our previous example, you thought of displaying pictures from both the Voyager and the Apollo side by side and for that you are querying the NASA Api, each request is async which means they'll return in their own time. But you want to display the data for both of them at the same time. Your basic instinct would be to make the first http request and inside that subscription make the second request.
  
  {% gist 2f7a592cd6f741f7391d813298392c34 nested-http-requests.js %}  
  
  Nested subscriptions will work but they might create timing and memory leak errors, which is the last thing you might want to deal with. They are not very easy to read, debug as well. We should use the forkJoin operator to make both requests in parallel and wait for them until both of them arrives. 
  
  {% gist 2f7a592cd6f741f7391d813298392c34 forkjoin-example.js %}  
  
  Forkjoin will take both of your observables, subscribe to each of them and once both have been fired, it will return you a new observable by combining both results. You can assume this as being similar to `Promise.all()`
  
  > Warning: If any one of the inner observables fails or doesn't emits, forkJoin will never emit a value. 
  
 <h3>Epilogue</h3> 
  
If you have made it this far, I have to thank you first, I am still new to writing blog posts on my own and I would love any feedback. I am open for any RxJs horror stories as well in your codebase. Kindly let me know your thoughts in the comments.
