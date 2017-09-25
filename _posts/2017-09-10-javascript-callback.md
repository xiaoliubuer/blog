---
layout: post
title:  "Javascript callback"
date:   2017-09-10 15:16:10 -0400
categories: articles
---

Original From: [here](http://javascriptissexy.com/understand-javascript-callback-functions-and-use-them)


In JavaScript, functions are first-class objects; that is, functions are of the type Object and they can be used in a first-class manner like any other object (String, Array, Number, etc.) since they are in fact objects themselves. They can be “stored in variables, passed as arguments to functions, created within functions, and returned from functions".

Because functions are first-class objects, we can pass a function as an argument in another function and later execute that passed-in function or even return it to be executed later. 

This is the essence of using callback functions in JavaScript. In the rest of this article we will learn everything about JavaScript callback functions. Callback functions are probably the most widely used functional programming technique in JavaScript, and you can find them in just about every piece of JavaScript and jQuery code, yet they remain mysterious to many JavaScript developers. The mystery will be no more, by the time you finish reading this article.

Callback functions are derived from a programming paradigm known as functional programming.

Consider this common use of a callback function in jQuery:

```javascript
$("#btn_1").click(function() {
  alert("Btn 1 Clicked");
});
```
Originally, when we want to call different function, we can use if conditions to return different value like below.

```javascript
let calc = function(num1, num1, calcType){
	if (calcType === "add"){
		return num1 + num2;
	}
	else if (calcType === "mulitply"){
		return num1 * num2;
	}
};

console.log(calc(2,3,'add'));
```

### How Callback Functions Work?
We can pass functions around like variables and return them in functions and use them in other functions. When we pass a callback function as an argument to another function, we are only passing the function definition. We are not executing the function in the parameter. In other words, we aren’t passing the function with the trailing pair of executing parenthesis () like we do when we are executing a function.


#### Callback Functions Are Closures
When we pass a callback function as an argument to another function, the callback is executed at some point inside the containing function’s body just as if the callback were defined in the containing function.

#### Basic Principles when Implementing Callback Functions

Use Named OR Anonymous Functions as Callbacks
In the earlier jQuery and forEach examples, we used anonymous functions that were defined in the parameter of the containing function. That is one of the common patterns for using callback functions. Another popular pattern is to declare a named function and pass the name of that function to the parameter. Consider this:

```javascript
// global variable​
​var allUserData = [];
​
​// generic logStuff function that prints to console​
​function logStuff (userData) {
    if ( typeof userData === "string")
    {
        console.log(userData);
    }
    else if ( typeof userData === "object")
    {
        for (var item in userData) {
            console.log(item + ": " + userData[item]);
        }
​
    }
​
}
​
​// A function that takes two parameters, the last one a callback function​
​function getInput (options, callback) {
    allUserData.push (options);
    callback (options);
​
}
​
​// When we call the getInput function, we pass logStuff as a parameter.​
​// So logStuff will be the function that will called back (or executed) inside the getInput function​
getInput ({name:"Rich", speciality:"JavaScript"}, logStuff);
​//  name: Rich​
​// speciality: JavaScript
```


#### Pass Parameters to Callback Functions
Since the callback function is just a normal function when it is executed, we can pass parameters to it. We can pass any of the containing function’s properties (or global properties) as parameters to the callback function. In the preceding example, we pass options as a parameter to the callback function. Let’s pass a global variable and a local variable:

```javascript
//Global variable​
​var generalLastName = "Clinton";
​
​function getInput (options, callback) {
    allUserData.push (options);
​// Pass the global variable generalLastName to the callback function​
    callback (generalLastName, options);
}
```

#### Make Sure Callback is a Function Before Executing It

It is always wise to check that the callback function passed in the parameter is indeed a function before calling it. Also, it is good practice to make the callback function optional.

Let’s refactor the getInput function from the previous example to ensure these checks are in place.

```javascript
function getInput(options, callback) {
    allUserData.push(options);
​
    // Make sure the callback is a function​
    if (typeof callback === "function") {
    // Call it, since we have confirmed it is callable​
        callback(options);
    }
}
```
Without the check in place, if the getInput function is called either without the callback function as a parameter or in place of a function a non-function is passed, our code will result in a runtime error.


#### Problem When Using Methods With The this Object as Callbacks
When the callback function is a method that uses the this object, we have to modify how we execute the callback function to preserve the this object context. Or else the this object will either point to the global window object (in the browser), if callback was passed to a global function. Or it will point to the object of the containing method.
Let’s explore this in code: 
```javascript
// Define an object with some properties and a method​
​// We will later pass the method as a callback function to another function​
​var clientData = {
    id: 094545,
    fullName: "Not Set",
    // setUserName is a method on the clientData object​
    setUserName: function (firstName, lastName)  {
        // this refers to the fullName property in this object​
      this.fullName = firstName + " " + lastName;
    }
}
​
​function getUserInput(firstName, lastName, callback)  {
    // Do other stuff to validate firstName/lastName here​
​
    // Now save the names​
    callback (firstName, lastName);
}
```
In the following code example, when clientData.setUserName is executed, this.fullName will not set the fullName property on the clientData object. Instead, it will set fullName on the window object, since getUserInput is a global function. This happens because the this object in the global function points to the window object.

```javascript
getUserInput ("Barack", "Obama", clientData.setUserName);
​
console.log (clientData.fullName);// Not Set​
​
​// The fullName property was initialized on the window object​
console.log (window.fullName); // Barack Obama
```
In the following code example, when clientData.setUserName is executed, this.fullName will not set the fullName property on the clientData object. Instead, it will set fullName on the window object, since getUserInput is a global function. This happens because the this object in the global function points to the window object.

```javascript
getUserInput ("Barack", "Obama", clientData.setUserName);
​
console.log (clientData.fullName);// Not Set​
​
​// The fullName property was initialized on the window object​
console.log (window.fullName); // Barack Obama
```

#### Use the Call or Apply Function To Preserve this

We can fix the preceding problem by using the Call or Apply function (we will discuss these in a full blog post later). For now, know that every function in JavaScript has two methods: Call and Apply. And these methods are used to set the this object inside the function and to pass arguments to the functions.

Call takes the value to be used as the this object inside the function as the first parameter, and the remaining arguments to be passed to the function are passed individually (separated by commas of course). The Apply function’s first parameter is also the value to be used as the this object inside the function, while the last parameter is an array of values (or the arguments object) to pass to the function.

This sounds complex, but lets see how easy it is to use Apply or Call. To fix the problem in the previous example, we will use the Apply function thus:

```javascript
//Note that we have added an extra parameter for the callback object, called "callbackObj"​
​function getUserInput(firstName, lastName, callback, callbackObj)  {
    // Do other stuff to validate name here​
​
    // The use of the Apply function below will set the this object to be callbackObj​
    callback.apply (callbackObj, [firstName, lastName]);
}
```
With the Apply function setting the this object correctly, we can now correctly execute the callback and have it set the fullName property correctly on the clientData object:

```javascript
// We pass the clientData.setUserName method and the clientData object as parameters. The clientData object will be used by the Apply function to set the this object​
 getUserInput ("Barack", "Obama", clientData.setUserName, clientData);
​
​// the fullName property on the clientData was correctly set​
console.log (clientData.fullName); // Barack Obama
```

We would have also used the Call function, but in this case we used the Apply function.

#### Multiple Callback Functions Allowed
We can pass more than one callback functions into the parameter of a function, just like we can pass more than one variable. Here is a classic example with jQuery’s AJAX function:

```javascript
function successCallback() {
    // Do stuff before send​
}
​
​function successCallback() {
    // Do stuff if success message received​
}
​
​function completeCallback() {
    // Do stuff upon completion​
}
​
​function errorCallback() {
    // Do stuff if error received​
}
​
$.ajax({
    url:"http://fiddle.jshell.net/favicon.png",
    success:successCallback,
    complete:completeCallback,
    error:errorCallback
​
});
```

