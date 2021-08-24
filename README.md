# Callback-promise-await-async
Callback-promise-await-async working logic with syntax and examples.

JavaScript is single threaded, blocking and synchronous by default. Operations will
always execute in a synchronous way. This means that JavaScript cannot handle 
concurrent requests and will execute the code sequentially, not parallelly. 
It is easy to think that JavaScript is only an imperative and object-oriented 
language, but it is actually a functional/imperative hybrid and an event-based 
language that provides object-oriented features via “prototypical inheritance”.
If you're executing a JavaScript block of code on a page then no other JavaScript on 
that page will currently be executed. JavaScript is only asynchronous in the sense 
that it can make, for example, Ajax calls.

Synchronous means the execution happens in a single event. It will only execute the 
next event once the previous event is finished.
In Asynchronous, the execution will never wait to complete, instead, it will execute 
all the events in a single go and multiple events will be in progress, hence making 
JavaScript non-blocking.

One important thing to keep in mind is that the single-threaded event handling 
systems are usually implemented using an event or message queue. So when a program is 
being executed synchronously, the thread will wait until the first statement is 
finished to jump to the second one, while in asynchronous execution, even if the 
first one was not completed, the execution will continue.

There are different ways to handle the async code. Those are callbacks, promises, 
and async/await.

---------------------------------------------------------------------------------------------------------------
Callbacks

In JavaScript, functions are objects. So we can pass objects to functions as 
parameters.We can also pass functions as parameters to other functions and call them 
inside the outer functions. So callback is a function that is passed to another 
function. When the first function is done, it will run the second function.

Let's take an example of callback function:

1.
function printString(){
   console.log("Anirban"); 
   setTimeout(function()  { console.log("Rajib"); }, 400); 
  console.log("Mohit")
}

printString();

or, as an arrow function:

function printString(){
   console.log("Anirban"); 
   setTimeout(() =>  { console.log("Rajib"); }, 400); 
  console.log("Mohit")
}

printString();


// OUTPUT
If that were sync code, we would have encountered the following output:

Anirban
Rajib
Mohit

But the setTimeout is an async function then the output of the above code will be:

Anirban
Mohit
Rajib

There is a built-in method in JavaScript called “setTimeout”, which calls a function 
or evaluates an expression after a given period of time (in milliseconds). In other 
words, the message function is being called after something happened (after 3 seconds
passed for this example), but not before. So the callback is the function that is 
passed as the argument to setTimeout.

2.
function getUser(){
           setTimeout(() => {
               const userids = [10, 20, 30, 40];
               console.log(userids);
               setTimeout((id) => {
                   const user = {
                       name:'John Doe',
                       age: 25
                   };
                       console.log(`User ID : ${id} : User name : ${user.name}, User Age: ${user.age}`)
                       setTimeout((age) => {
                           console.log(user);
                       }, 1000, user.age);
           }, 1000, userids[3]);
           }, 1500)
       }
 getUser();

//OUTPUT

(4) [10, 20, 30, 40]
User ID : 40 : User name : John Doe, User Age: 25
{name: "John Doe", age: 25}

As you can see in this example, we have three callbacks nested inside one another, 
which means three chained ajax calls to get the data from the server. It might have 
more and more chained callback functions and this might get out of hand. This concept 
is called “Callback hell” in JavaScript.

----------------------------------------------------------------------------------------------------------
Promises

A promise is an object which keeps track of whether the asynchronous event has 
happened already or not and determines what happens after the event has occurred. 
It provides two values, resolved or rejected. It can be in three states, fulfilled, 
pending or rejected. It helps to catch all the errors that occurred after rejection 
or attach a callback to the handle of the fulfilled value.
Before the event has happened the promise is pending, then after the event has 
happened it is called resolved. When the promise is successful and the result is 
available, then it will be fulfilled, but if it caught errors, then it will be called 
rejected.

States of Promises:

First of all, a Promise is an object. There are 3 states of the Promise object:

Pending: Initial State, before the Promise succeeds or fails.
Resolved: Completed Promise
Rejected: Failed Promise, throw an error
For example, when we request data from the server by using a Promise, it will be in 
pending mode until we receive our data.

If we achieve to get the information from the server, the Promise will be resolved 
successfully. But if we don’t get the information, then the Promise will be in the 
rejected state.

Creating a Promise:

Firstly, we use a constructor to create a Promise object. The promise has two 
parameters, one for success (resolve) and one for fail (reject):

Syntax: const myPromise = new Promise((resolve, reject) => {  
        // condition
       });

Example:

const myFirstPromise = new Promise((resolve, reject) => { 
    const condition = true;   
    if(condition) {
         setTimeout(function(){
             resolve("Promise is resolved!"); // fulfilled
        }, 300);
    } else {    
        reject('Promise is rejected!');  
    }
});
In the above Promise If Condition is true, resolve the promise returning the 
“Promise is resolved ”, else return an error “Promise is rejected”. Now we have 
created our first Promise, Now let's use it.

Using Promise:

To use the above create Promise we use then() for resolve and catch() for reject.

myFirstPromise
.then((successMsg) => {
    console.log(successMsg);
})
.catch((errorMsg) => { 
    console.log(errorMsg);
});
let's take this a step further:

const demoPromise= function() {
  myFirstPromise
  .then((successMsg) => {
      console.log("Success:" + successMsg);
  })
  .catch((errorMsg) => { 
      console.log("Error:" + errorMsg);
  })
}

demoPromise();
In our created promise condition is “true” and we call demoPromise() then our console 
logs read:

Success: Promise is resolved!
So if the promise gets rejected, it will jump to the catch() method and this time we 
will see a different message on the console.

Error: Promise is rejected!

Sometimes we need to call multiple asynchronous requests, then after the first 
Promise is resolved (or rejected), a new process will start to which we can attach it 
directly by a method called chaining.

So we create another promise:

const helloPromise  = function() {
  return new Promise(function(resolve, reject) {
    const message = `Hi, How are you!`;

    resolve(message)
  });
}
We chain this promise to our earlier “myFirstPromise” operation like so:

const demoPromise= function() {

  myFirstPromise
  .then(helloPromise)
  .then((successMsg) => {
      console.log("Success:" + successMsg);
  })
  .catch((errorMsg) => { 
      console.log("Error:" + errorMsg);
  })
}

demoPromise();
Since our condition is true, the output to our console is:

Hi, How are you!
Once the “hello” promise is chained with .then, subsequent .then utilizes data from 
the previous one.

------------------------------------------------------------------------------------------------------------
Async/Await:

Await is basically syntactic sugar for Promises. It makes your asynchronous code look 
more like synchronous/procedural code, which is easier for humans to understand.
Async/Await is used to work with promises with asynchronous functions.

 Putting Async in front of the function expects it to return the promise. 
 This means all async function have a callback.
 Await can be used for single promises to get resolved or rejected and return the 
 data or error.
 Async/Await behaves like synchronous code execution.
 There can be multiple awaits in a single async function.
 We will be using try/catch construct, which makes async/await easy to handle 
 synchronous and asynchronous code.
 Async/Await helps you to deal with callback hell.

Syntax of Async and Await:

async function printMyAsync(){
  await printString("one")
  await printString("two")
  await printString("three")
}

You can see that we use the “async” keyword for the wrapper function printMyAsync. 
This lets JavaScript know that we are using async/await syntax, and is necessary if 
you want to use Await. This means you can’t use Await at the global level. It always 
needs a wrapper function. Or we can say await is only used with an async function.

The await keyword is used in an async function to ensure that all promises returned 
in the async function are synchronized, ie. they wait for each other. Await eliminates 
the use of callbacks in .then() and .catch(). In using async and await, async is 
prepended when returning a promise, await is prepended when calling a promise. try
and catch are also used to get the rejection value of an async function.

Example:
1.
async function demoPromise() {
  try {
    let message = await myFirstPromise;
    let message  = await helloPromise();
    console.log(message);

  }catch((error) => { 
      console.log("Error:" + error.message);
  })
}

// finally, call our async function

(async () => { 
  await myDate();
})();

2.
const userfunc = async () => {
          try {
              const id = await getuserIds;
              console.log(id);
              const getusername = await getUsername(id[3]);
              console.log(getusername.username);
              const getuserage = await getUserage(id[3]);
              console.log(getuserage.user_age);
          } catch (error) {
              console.log(error);
          }
      };
      userfunc();

//OUTPUT
(4) [10, 20, 30, 40]
Promise {<pending>}
John Doe
25

--------------------------------------------------------------------------------------------------------
  Conclusion

In this article, we have learned the difference between synchronous and asynchronous 
JavaScript, and also learned how to use a callback, promises and async/await.
