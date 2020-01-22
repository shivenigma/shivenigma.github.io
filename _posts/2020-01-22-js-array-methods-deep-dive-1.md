---
layout: post
title: "A Deep Dive into Javascript Array Methods - 1"
date: 2020-01-22
excerpt: "Starting with programming can be both exciting and exhausting at the same time, its a demanding activity that needs a lot of focus and energy to do it correct and consistent.

I am going to share 10 mistakes I think most beginners make and some advice on how to navigate away from those mistakes and accelerate your learning journey and career."
tags: [beginner, javascript, array, advanced, looping, modifying]
published: false
featured: "/assets/img/posts/beginner-mistakes/10.jpg"
---

Javascript is great, it is single-threaded but fast enough for most use cases, it is easy to get started with but hard to master. 2019 has been great for Javascript and 2020 is not going to be any different. In this context, I am focusing more on writing beginner-friendly articles on Javascript this year.

In Javascript, we mainly use two data types arrays, and objects. Technically arrays are also objects in javascript, they have some prototype methods for traversing and mutating arrays, more on that is for another blog post. To work with arrays javascript has a lot of methods and sometimes it can be overwhelming to figure out which method does what and what arguments it takes, whether it modifies the original array or it creates and returns a new array. 

In this post, we are going to approach some of the most used array methods and I tried to approach them and write about them in a way that it sticks. I have also seen people use different methods for different purposes, like using map/filter to loop over an array, it works but that's not it meant to be used for. Hopefully, after reading this post you should be able to pick up any trivial use case and work with the right JS method for it.

I originally planned to write this as a single blog post but after seeing the amount of information need to be put for each array method, I split it into a series of blog post, this will be the first part of that series.

## 1) You want to loop over an array (forEach)

array.forEach is the semantically correct method for this purpose, I have seen many people use map as well, it works but map means that you're somehow modifying the original array and forEach means you don't.

When I say about modifying an array, I am not saying that it is not possible, I rather mean it should be avoided. To keep your code clean and to easily let other devs know your intention of a code block.

    const names =["Javascript", "Python", "PHP", "Flutter"];
    names.forEach(name => {
    	console.log(name);
    });

The above code loop over an array and prints each element in the console. This is a very basic implementation of forEach.

### Method Signature

    arr.forEach(callback, [, thisArg]);
    
    // thisArg is optional for forEeach
    
    // the callback has the following arguments in forEach.
    
    callback(currentValue, [, index [, array]])
    // index and array are optional

**callback -** The function which will be called once for each element in the array.

**currentValue** - The current element that is being iterated in the array

**index** (optional) - The index of the current element in the array

**array** (optional) - The array that is being traversed

**thisArg -** This is interesting before ES6 arrow functions. you can specify something that can be used as this inside the callback function. If you don't pass thisArg, the value of this will be determined as to [how a function's this will be determined](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/this).

### Things to remember

- forEach does not mutate the array which is being traversed, but the callback can do it. (I recommend not doing it for obvious reasons mentioned down below).
- If the array is modified when forEach is running, some elements might be skipped as forEach doesn't make copy of the array [(Example)](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/forEach#If_the_array_is_modified_during_iteration_other_elements_might_be_skipped.).
- Elements which are appended after the forEach has started will not be traversed.
- undefined, uninitialized values in the array will not be traversed in forEach.
- forEach is not chainable because it returns undefined.
- Early termination out of forEach is not possible, you can only throw an exception. If you need early termination, use for or for.. in loop.
- If callback is an arrow function, thisArg can be ignored as arrow functions lexically bind to this value.

## 2) You want to transform an array in some way

Transforming means that you're creating a new array by applying some sort of transformation to each element in the original array. let's add numbers to the languages array we used in our previous forEach example.

    const names =["Javascript", "Python", "PHP", "Flutter"];
    const newNames = names.map((name, index) => {
    	return index.toString() + '. ' +name;
    });
    console.log(newNames);
    // OUTPUT : Array ["0. Javascript", "1. Python", "2. PHP", "3. Flutter"]

You should not use map in either of the below cases,

1. You're not using the newly created array it returns
2. You're not returning anything from the callback.

In both cases above, you're probably trying to do something else which map is not the right way.

### Method Signature

    let newArray = arr.map(callback, [, thisArg]); // thisArg is optional
    callback(currentValue, [, index, [, array]]); // index and array are optional
    
    the callback should return an element for the new array after applying any transformation

**callback -** The function which will be called once for each element in the array.

**currentValue** - The current element that is being iterated in the array

**index** (optional) - The index of the current element in the array

**array** (optional) - The array that is being traversed

**thisArg -** This is same as we saw for forEach, used to refer the this value inside callbak.

Map receives a function as the first argument and an optional index as the second argument, in the callback function you'll receive each item in the array as an argument and you must return the element after applying your transformation. 

### Things to remember

- the map does not mutate the array which is being traversed, but the callback can do it. (In short, don't do it to avoid unintended consequences).
- Elements which are appended after the map has started will not be traversed.
- map is chainable with other array methods
- Early termination out of map is not possible. If you need early termination, you're using map in the wrong place.
- The callback must return value otherwise the result array will be array of undefined values.
- If callback is an arrow function, thisArg can be ignored as arrow functions lexically bind to this value.

These two are some of the frequent use cases I will come across in day to day work. I tried to write it simple as possible to my understanding and I hope you find this useful. If you think if something has to be added to the post feel free to let me know in the comments. Try to play with these two methods a little bit if you find them difficult or confusing. Remember, practice is what makes things stick in your memory not just reading.
