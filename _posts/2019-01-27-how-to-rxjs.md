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
 
 - It treats any even or input as a stream which can flow through various observables, operators.
 
 When I started with rxjs I had no clue on how or when to use those operators it has, because it was not very obvious without understanding the above two things. Nowadays I think I am doing fine and if you're on the same boat I would like to show you some practical use cases of where you can use one operator instead of doing something else.
 
 