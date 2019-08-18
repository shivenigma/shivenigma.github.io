---
layout: post
title: "A Javascript Interview question that no one answered"
date: 2018-12-25
excerpt: "Tricky javascript question that can be answered only if you know JS well enough."
tags: [javascript, interview, js, scoping, hoisting]
published: false
featured: "/assets/img/posts/hoisting/unsplash-stress.jpg"
---
<figure>
	<img src="/assets/img/posts/hoisting/unsplash-stress.jpg">
</figure>

Javascript is a beautiful language which comes bundled with some very different behavior, which can be tricky understand even for people who have been working in JS for years. There are even multiple books were written to address those special behavior such as the [You don't know JS series](https://www.amazon.in/You-Dont-Know-Set-Volumes/dp/9352136268) by [Kyle Simpson](https://me.getify.com).

In my [workplace](https://www.agiratech.com/) I have had the pleasure of interviewing quite a lot people for Javascript and Angular positions. I don't like to ask them to do whiteboard or Fizzbuzz tests, instead I like to see is how a person analyze a problem/code when being under interview pressure. So more than often I write a piece of puzzling (even broken) JS code in-front of them and will ask them to look at the code and write the output and explain the reason to me.

In that line of problems, there is a favorite question of mine, which a lot of people misunderstood and answered incorrectly. If my memory serves right only 2 out of 20+ people answered correctly (Sorry the title was a click-bait and people have answered it before) and was able to explain the reason as well. Let's see the problem and then discuss the output and the reason of the behavior.

{% gist eff8a0e4b4f9ec53e31b1827d017c645 question1.js %}

The above code is pretty simple with few assignments and log statements, think of what will be the output of the code and check your answer with the screenshot below.

 ![Console output](/assets/img/posts/hoisting/console_output.png)

Most people will either miss the `undefined` or will fail to explain why it is `undefined`. If your answer is right and you know why this is happening, you probably can move on without reading further (But please read it anyway and give me your feedback).

###Variable scopes

There are two interesting things happening here, first one is how js resolves variable definitions when encountered a variable in the code. We defined a global variable name and then immediately put a `console.log` for it. After that it encounters a function definition and then another `console.log` in global scope. In both cases the JS interpreter will look for a variable defined as `name` in the current scope which is the global scope in this case.

> When the JS interpreter encounters a variable usage it'll look in the current scope and if the variable isn't defined in the current scope, it will look in the parent scope and grandparent's and so on, until it either find the variable or it reaches the global scope. If the variable is not found in any scopes, it'll be returned as undefined.

In our code we have a function defined as `print()` which also put `console.log` for the `name` variable, so according to the above quoted lines, the print function should look for the name variable in its current scope and which is undefined at the moment and it should reach the global scope and should write the value `'Angular'` in the console. But that's not the case here because of another interesting approach javascript takes.

### Hoisting

Hoisting is a javascript behavior which moves function and variable declarations to the top of the scope where they're defined. Declaring a variable and initializing it with a value are two different things which happens at two different phase in javascript no matter how or where you define them in your code. For example,
{% gist eff8a0e4b4f9ec53e31b1827d017c645 hoisting.js %}

Our print function is declaring 3 variable at a later stage after the console log but the js interpreter will move the declarations to the top of the current scope which is the equivalent of the second function in the above gist. This is exactly what is happening in our initial problem.

When the `print()` function defines a variable `var name = "React"` the `var name;` will be moved to the top and when JS interpreter looks for the variable within the function's scope, it'll be available but the value will not be assigned to it, hence the log says `undefined` because it is undefined at the moment within the function's scope. In the second `console.log` you can see it logs the value fine because the value has been assigned to the variable once that assignment code is executed.

I hope I explained it as best as I can, feel free to let me know of any feedback/queries in the comments. 

> Note: Asking only this kind of tricking questions in an interview is not always the ideal approach to see the best of a candidate. I am mostly using this to see if the candidate is able to think like a compiler/interpreter when encountered with a tricky issue. You should probably use tricking questions along with other proven interview practices to get the best outcome.
