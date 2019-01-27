---
layout: post
title: "You're probably doing rxjs wrong"
date: 2019-01-27
excerpt: "How to take advantage of RxJs?"
tags: [angular, rxjs, RxJs, javascript, Observable, operators, subscription, streams]
---
I have to agree that the above title is pretty strong and kinda click-a-bait. But trust me, I was using the library wrong, I was not utilizing it to its full extent and was writing code which should be far more easier.

To understand the problem behind this let's get to the background, if you're like me, you're probably forced into rxjs by Angular and you were just using it only to subscribe to http calls and not even bothered to look at its documentation (Looking at you fellow Indian devs). z