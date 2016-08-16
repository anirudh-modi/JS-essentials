# Prototype methods

In ES6, a bunch of methods were introduced in `Array.Prototype` so that all the arrays can use the methods :

1. `copyWithin`
2. `find`
3. `fill`
4. `findIndex`
5. `entries`
6. `values`
7. `keys`

## `copyWithin` prototype method

The `copyWithin` method is used to copy a part of an array, which then replaces the entire content of the same array. It accepts 3 parameters :

1. target - it is the index at which the copying must begin.
2. start - it is the index in the array, from which copying must start, by default it is 0. (this index will be included)
3. end - it is the index where the copying must stop (this index will be excluded), by default it is the length of the array.

Let's look at some example to see what these will really do.

````javascript
[0, 1, 2, 3, 4, 5].copyWithin(2);					// [0, 1, 0, 1, 2, 3]

[0, 1, 2, 3, 4, 5].copyWithin(2, 1);			// [0, 1, 1, 2, 3, 4]

[0, 1, 2, 3, 4, 5].copyWithin(2, 2);			// [0, 1, 2, 3, 4, 5]

[0, 1, 2, 3, 4, 5].copyWithin(2, 0, 1);		// [0, 1, 0, 3, 4, 5]
````

Now if look at the above example, the target index is `2`, which means that the whatever element which needs to be copied, will be copied form the target index.

Since, we didn't specified the start index, by default it will be `0`, implying, the elements from 0th index will be taken as the start point to copy. And the end was also not specified which means the n-1 in this case 5th index will be excluded.

The loop starts from start `0` until the end `length` of the array, it takes 0th element and inserts it in the 2nd index, then it takes 1st index and inserts in 3rd index, then its takes the 3rd index and inserts in the 4th index, and it then takes the 3rd index and inserts it at the 5th pos. *The end and the start is used to determine the number of times loop will be executed to fill the new array*.

All the above index can also be negative.
1. If target is negative, it is treated as *length + target* where length is the length of the array.
2. If start is negative, it is treated as *length + start*.
3. If end is negative, it is treated as *length + end*.

````javascript
[0, 1, 2, 3, 4, 5].copyWithin(-1);					// [0, 1, 2, 3, 4, 0]
// Equivalent to : [0, 1, 2, 3, 4, 5].copyWithin(5);

[0, 1, 2, 3, 4, 5].copyWithin(1, -3);				// [0, 3, 4, 5, 4, 5]
// Equivalent to : [0, 1, 2, 3, 4, 5].copyWithin(1, 3);

[0, 1, 2, 3, 4, 5].copyWithin(1, 3, -2);		// [0, 3, 2, 3, 4, 5]
// Equivalent to : [0, 1, 2, 3, 4, 5].copyWithin(1, 3, 4);
````

## `find` and `findIndex` prototype method
The `find` and `findIndex` method accepts a function as a callback function which is executed on every element present in the array. and an optional value to use as `this` inside the callback function. The callback function accepts three parameters :

1. element - this signify the current element on which the function will be executed
2. index - the index of the current element
3. array - the original array

Prior to ES6 the `indexOf` methods use to only give the index, if a value is present in the array, however the element was never returned, and also for a more complex array, the `indexOf` method was not of much use, so to solve these, the `find` and `findIndex` was introduced.
Both the method will return the first element found in the array.

````javascript
var arr = [ {name : 'Anirudh'}, {name : 'Modi'}, {name : 'Mat'}, {name : 'John'} , {name : 'Anirudh'}];

arr.find(element => element.name === 'Anirudh');

//{ name : 'Anirudh' }

arr.findIndex(element => element.name === 'Mat');

// 2
````

One more issue with the `indexOf` method was that it was never able to find the `NaN`, and this can be solved using the `Object.is` along with the `find` and `findIndex`.

````javascript
[NaN,1].indexOf(NaN)
// -1

[NaN,1].findIndex(element => Object.is(NaN,element));
//0
````

## `fill` prototype method

The fill method can be used to fill an existing array partially or completely, the `fill` method accepts three parameters :
1. value, with which the array must be filled.
2. start, optional defaults to 0, is the index position from which the filling must start
3. end, optional defaults to array.length, is the index position till which the filling must be done, this index will be excluded

````javascript
[1, 2, 3].fill(4);		// [4, 4, 4]

[1, 2, 3].fill(4, 2);		// [1, 2, 4]

[1, 2, 3].fill(4, 1, 2);		// [1, 4, 4]
````

If start is negative, then length+start will be considered as start position, and if he end is negative, then length+end will be considered as end position.

````javascript
[0, 1, 2, 3, 4].fill(5, -1);		// [0, 1, 2, 3, 5]

[0, 1, 2, 3, 4].fill(5, 1, -1);		// [0, 5, 5, 5, 4]
````

## `value`, `entries` and `keys` prototype methods

The `value` `entries` and `keys` method return an Array iterator, which can be used to iterate over any array using `next` on the array iterator.

The `value` method will create and return an iterator from the values of the array.

The `keys` method will create and return an iterator from the keys in the array.

The `entries` method will create and return an iterator which has key/value pair present in the array.

````javascript
var arr = ['a', 'b'];

var arrIteratorEntry = arr.entries();
var arrIteratorVal = arr.values();
var arrIteratorKey = arr.keys();

console.log(arrIteratorEntry.next().value);	// [ 0, 'a' ]
console.log(arrIteratorEntry.next().value);	// [ 1, 'b' ]

console.log(arrIteratorVal.next().value);	//  'a'
console.log(arrIteratorVal.next().value);	//  'b'

console.log(arrIteratorKey.next().value);	//  0
console.log(arrIteratorKey.next().value);	//  1
````
