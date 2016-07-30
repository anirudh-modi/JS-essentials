# Static methods to create new Array

Prior ES6, there were two of creating a new array, one using the array literal `[]`, the second was using the `Array` constructor. Creating an array using the `Array` constructor has been a little less intuitive.

````javascript
var arr1 = new Array(2);
````

If we look at the above code, one might think that an array will be created with element `2` in it, `[2]` is expected, however, the `Array` constructor will create an array with a length of `2`, and two empty slots will created, `[undefined x 2]` in Chrome.

The `Array` constructor, accepts n-number of parameters, if the only one parameter is passed then, the first parameter in `Array` constructor behaves a little differently, it will first check whether the parameter is number or a string, if the  it is a number, then it will create an array, with n-number of empty slots, and the length of the array will the number specified, else if it is a string then an array of length `1` is created and the first element will the string specified.

````javascript
var arr1 = new Array('red');
console.log(arr1);        // ['red']

var arr2 = new Array(5);
console.log(arr2);        // [undefined x 5]
````

However if we are passing more than one parameter, then all the parameter are pushed into the array as its element, irrespective of whether the first parameter is a number or not.

````javascript
var arr1 = new Array(3,'red');
console.log(arr1);        // [3,'red']
````

This, behavior prevents us from creating an array with just a single number it, if we want to use the `Array` constructor, so to overcome, two new static function was included, in `Array` constructor, which can be used to create a new array.

### `Array.of`

The `Array.of` is not supposed to have the issues which was with the `Array` constructor, it accepts n-number of parameter, which are used to creates an array. If no parameter is passed then an empty array is returned.

````javascript
var arr1 = Array.of(1);
console.log(arr1);      // [1]

var arr2 = Array.of(1, 3, 'red');
console.log(arr2);      // [1, 3, 'red']

var arr1 = Array.of();
console.log(arr1);      // []
````

### `Array.from`

The `Array.of` can have one drawback, which is if we wish to create an array out of an [`array-like` object](http://www.2ality.com/2013/05/quirk-array-like-objects.html) (`node-list`,`arguments`,etc..) or an [`iterable` object](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Iteration_protocols#Builtin_iterables), we will have to manually run and loop and create an array out of it.

However, the `Array.from` method serves the exact drawback, and also gives us a power to change the type of the element in the array, by giving a mapping function. Let's first see the basic.

The `Array.from` accepts 3 parameters :
1. The `iterable` or `array-like` object which needs to be converted into array.
2. The mapping function, which is an optional parameter, which can be used to what goes in the array. If this function is not provided then no mapping will be applied on the individual element of the object.


#### `array-like` object
````javascript
function foo() {
  var argsArray = Array.from(arguments);
  console.log(argsArray);                 //  [ 1 , 2 , 3 , 4 , 5 , 6 ]
}

foo( 1 , 2 , 3 , 4, 5 , 6 );
````

#### `iterable` object

````javascript
var str = 'red';
var redArray = Array.from(str);
console.log(redArray);            // [ 'r', 'e', 'd' ]

var set = new Set();

set.add(1);
set.add(5);

var arraySet = Array.from(set);
console.log(arraySet);          // [1 , 5]
````

#### Using `map` in `Array.from`

We watched the basic example of how we can convert any `array-like` or `iterable` object into an array. Now, a lot of the objects we wish to convert are not as simple as number, it might be array of objects, or we might even want to perform some operation on the original element and then create an array out of it.

Prior to ES6, we need to first covert the object into an array and then we can use the `.map` function on the array to get new array. Something like this.

````javascript
function foo() {
  var argsArray = Array.prototype.slice.call(arguments)

  var mappedArray = argsArray.map(function(element) {
      return element + 1;
  });

  console.log(mappedArray);                 //  [2, 3, 4, 5, 6, 7]
}

foo( 1 , 2 , 3 , 4, 5 , 6 );
````

With `Array.from` we can specify the map function which needs to applied on the element while creating the new array. The `Array.from` accepts a function as the second parameter, which specifies the mapping which needs to applied on every element of the original object.

````javascript
// For array-like objects
function foo() {
  var argsArray = Array.from(arguments, element => element + 1);

  console.log(argsArray);                 //  [2, 3, 4, 5, 6, 7]
}

foo( 1 , 2 , 3 , 4, 5 , 6 );

// For iterable objects
var set = new Set();
set.add(1);
set.add(5);
var arraySet = Array.from(set, element => element + 1);

console.log(arraySet);                    // [2, 6]
````

````javascript
function foo() {
  var idArray = Array.from(arguments, element => element.id);           // [1, 2, 3]
  var nameArray = Array.from(arguments, element => element.name);       // ['Goku', 'Vegeta', 'Picolo']
}

foo({id : 1, name : 'Goku'}, {id : 2 , name : 'Vegeta'}, {id : 3 , name : 'Picolo'});
````

The `Array.from` will internally first check whether the source is an `iterable` or `array-like` object. If it turns out to be an `iterable` then it will use `.next` of the `iterable` to get values from source object, and assign it to array.

````javascript
Array.from('');       // create an empty array []
Array.from('   ');    // [" ", " ", " "]
````

If the source object is found to be an `array-like` object it will first coerce the source to an `Object`, using `ToObject`, which means using `null` and `undefined` will give `TypeError`, if not then it will return the corresponding Object wrapper, then it will find the `length` of the source object, so if the length is `undefined` it return an empty array. Else it will try to create array with the length found. Let's look at an example below.

> __NOTE__ : The example below, is just to understand how `Array.from` works, and is not encouraged to use the example below to create array.

````javascript
Array.from(undefined);          // TypeError

Array.from(null);               // TypeError

Array.from(3);                  // []

Array.from(function(){});       // []

Array.from(function(a,b){});    // [undefined, undefined]
````

If we try to see, the first will return a `Number` wrapper, which wont have a `.length` of its own, and hence an empty object will be created, however when we pass a function, every function has a `.length` which actually specified the number of parameter defined, so for the first example it was 0, hence an empty array was returned however in the second example the function accepts two parameter, hence the `.length` will be 2, and hence an array with length of 2 was created.
