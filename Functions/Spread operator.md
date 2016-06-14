## Spread operator

The syntax for the `spread` operator is similar to that of the `rest` parameter i.e `...` , however, it performs the job of spreading an iterable, it behaves a little differntly depending on where it is being used. Let's have a look at how it works.

**During function and method call**

When using a `spread` operator during function call, the `spread` operator does the job of spreading the values mentioned in an array as the values to the parameters defined in function declaration.
````javascript
function foo(a,b,...restParam)
{
    console.log(a);             // 1
    console.log(b);             // 2
    console.log(restParam);  // [3,4]
}

foo(...[1,2,3,4]);
````

Let's take an example, on how it will behave in a real life scenario. We all know `array.push` method, it accepts n number of parameters, and for each value it pushes the value in the corresponding array, So when we use the `spread` operator, during calling the `array.push` method, the operator will spread all the values as the arguments for the `array.push`, for smaller chunks of `push` you might not find `spread` operator useful, however for a larger one, it can be more useful to push all the values in an array and put the `...` in front of the array and pass it directly to the `array.push`.

````javascript
var arr = [1,2];

arr.push(...[3,4,5]);

console.log(arr);    // 1,2,3,4,5

var arr2 = [6,7];

arr.push(...arr2);

console.log(arr);   // 1,2,3,4,5,6,7
````

Let's look at another scenario, suppose we want to create a function sum, which can accept n number of arguments and return the sum of all arguments.

ES6 Prior
````javascript
function sum()
{
    var sumAll=0;
    if(arguments.length)
    {
        for(var i=0;i<arguments.length;i++)
        {
            sumAll = sumAll+arguments[i];
        }
    }

    return sumAll;
}
sum(1);         // 1
sum(1,2);       // 3
sum(12,2,3,4);  // 21
````
 
 This function decalration takes care of multiple params, however suppose we want to send an array of numbers, and we want to achieve the same result, without modifying the content of sum function, then the way we call the function sum needs to be modified, that is we need use `apply` on the sum function, and pass the array, so the `apply` will pass the individual values of the array as the `arguments` for the function `sum`.

 ````javascript
 sum.apply(sum,[1,2,3]);    // 6
 ````

Now, the same thing can be used using the `spread` operator and the `rest` parameter, in ES6, lets see how it can be done.

````javascript
function sum(...rest)
{
    var sumAll=0;

    if(rest.length)
    {
        rest.forEach(function(i)
        {
            sumAll = sumAll+rest
        });
    }

    return sumAll;
}
// we can call like this
sum(1,2,3,4,5,6);       // 21

// or if we want to use an array then we can use the spread operator
sum(...[1,2,3,4,5]);    //15
````

**In arrays**

The `spread` operator is not just retricted to spreading values of an iterable during function call, it can be used within array also, meaning reconstructing a new array with an existing array. 

Lets, have a look on this can be acheived in ES5.

````javascript
var arr1=[2,3,4];
var arr2=[1];
// create an array 2 with values 1,2,3,4,5 using arr1

for(var i=0,len<arr1.length;i<len;i++)
{
    arr2.push(arr1[i]);
}
arr2.push(5);

//Or
arr2.concat(arr1, [5]);
````

We have to run a `for..loop` over the `arr1` then push values for every `arr1` value, or use `concat`. So the `spread` operator can solve this very simply and elegant way.

````javascript
var arr1 = [2,3,4];

var arr2 = [1,...arr1,5];

console.log(arr2);  // [1,2,3,4,5]
````

Not only the code is reduced, it reduces the effort to create an array.

Another, advantage of having the `spread` operator can be to `destructure` any `iterables`. Let's have a look what it means and how it can help. Let's consider if there is an array `[1,2,3,4,5]` and we want to spilt to variable `a` holding first value, and another array `b` holding rest of the values in array.

````javascript
var arr1 = [1,2,3,4,5];

var a = arr1[0];

var restArray = arr1.slice(1);

// Or

var restArray = [];

for(var i=1,len<arr1.length;i<len;i++)
{
    restArray.push(arr1[i]);
}
````

Even though nothing is wrong with it, but the same thing can achieved using `spread` operator in a cleaner way.

````javascript
var arr1 = [1,2,3,4,5];

[a,...restParam]=arr1;

console.log(a);             // 1

console.log(restParam);     // [2,3,4,5]
````

The above trick is known as `destructing` which we will cover later in more depth. One common exmaple/ practical exmaple of `spread` operator along with `destructing` could be using the `arguments` object.

> Even though we previously went thru the syntax of `rest` parameter, it is just to show how `spread` can also be used to `destructure` something.

````javascript
function foo(a,b)
{
    [a,b,...restParam] = arguments;
    console.log(restParam);         // [3,4]
}

foo(12,2,3,4);
````

**What do you mean by iterables?**

Now, throught the explaination I mentioned that the `spread` operator can be used over an **iterable**. So, what does this iterable means?

`Iterables` are objects which have the property `Symbol.iterator` as a method, Built in `iterable` in Js are `Array`,`String`,`Set`,`TypedArray`,`arguments` and so on. We can create our `iterable` object.

Because of this reason, the `arguments` object can be in `spread` operator.

You may find some where that `node-list` can also be converted into `array` using `speard` operator, however, this depends on browser, in firefox the `node-list` are considered as `iterable`, in safari and chrome they are not `iterable`. 

````javascript
var text = 'blue sky';
var splittedChars = [...text];
console.log(splittedChars);     // ["b", "l", "u", "e", " ", "s", "k", "y"]
                                // this works because the string is an
                                // iterable
````

****

**Previous Chapter : [Rest parameter](https://github.com/anirudh-modi/JS-essentials/blob/master/Functions/Rest%20parameter.md)**

**Next Chapter : [Arrow functions](https://github.com/anirudh-modi/JS-essentials/blob/master/Functions/Arrow%20functions.md)**
