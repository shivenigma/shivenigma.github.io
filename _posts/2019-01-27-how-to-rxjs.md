---
layout: post
title: "You're probably doing rxjs wrong"
date: 2019-01-27
excerpt: "How to take advantage of RxJs?"
tags: [angular, rxjs, RxJs, javascript, Observable, operators, subscription, streams]
---
I have to agree that the above title is pretty strong and kinda click-a-bait. But trust me, I was using the library wrong, I was not utilizing it to its full extent and was writing code which should be far more easier.

To understand the problem behind this let's get to the background, if you're like me, you're probably forced into rxjs by Angular and you were just using it only to subscribe to http calls and not even bothered to look at its documentation (Looking at you fellow Indian devs and my past self).

I hope few of us tried to use rxjs and tried push our limits but failed or gave up, why rxjs seems so hard at the beginning? Like every other thing, rxjs indeed has a learning curve to it but it isn't thing. Its our approach towards programming, most of us working in either procedural or OOP style and rxjs requires little different approach towards how you handle input and output.

 Rxjs has two distinct characteristics,
 
 - It uses pure functions to produce values and relies on them to keep the state error free.
 
 - It treats any event or input as a stream which can flow through various observables, operators.
 
 When I started with rxjs I had no clue on how or when to use those operators it has, because it was not very obvious without understanding the above two things. Nowadays I think I am doing fine and if you're on the same boat I would like to show you some practical use cases of where you can use one operator instead of doing something else.
 
 <h3>
 <a target="_blank" href="https://rxjs-dev.firebaseapp.com/api/operators/map">Map operator </a>
 </h3>
 
 Map mainly useful for transforming data. What it does is that every time your source observable fires, it takes the value and send it to the function you defined (should be pure function, please people) and then returns the result of that function as an observable.
 
 <h3>
 <a target="__blank" href="https://rxjs-dev.firebaseapp.com/api/operators/filter">Filter operator</a>
 </h3>

 Filter operator is similar to array filter, in fact all the operators are inspired by the Array functions in JS, so when your source observable emits a value, the filter takes the value and sends it to the predicate function you defined and if the function returns true it emits the observable.
 
 <h3>
 <a target="__blank" href="javascript:void(0);">forkJoin</a>
 </h3>
  
  Forkjoin is a combination of operators which can be used mostly to get rid of nested subscriptions. Imagine that you have two http requests which are not dependent on each other, but you need the result from both of them to perform some operation. Your basic instinct would be to make the first http request and inside that subscription make the second subscription. It will work but not the best way, The correct way would be to use a forkJoin and add both requests to it and treat them as a single observable.
  
  Forkjoin will take both of your observables, subscribe to each of them and once both of the results are arrived it will return you a new observable by combining both results. You can assume this as being similar to `Promise.all()`
  
  > Warning: If any one of the inner observables fails or doesn't emits, forkJoin will never emit a value. 
  
  <h3>
   <a target="__blank" href="javascript:void(0);">take</a>
   </h3>
    
  Imagine if you have an source (observable) which emits multiple values but you only care about the first `n` values. For example if a user clicks on a button and you're making a call for that, if you directly bind your html event to the http call, chances are multiple subsequent http calls. But we can use take(1) to prevent this as it'll emit subsequent clicks after the first click.
  
 <h3>
     <a target="__blank" href="javascript:void(0);">flatMap</a>
 </h3>
