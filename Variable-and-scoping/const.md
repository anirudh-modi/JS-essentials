## `const` declaration

Variables declared using `const` are considered constants, meaning their values cannot be changed once set. because of which, it becomes important that while declaring a `const`, we need to initialize the variable too, signifying a `const` is a read only variable  `const` can used as below :

````javascript
const a = 1;

const b; //missing intialization error

a = 2; // "a" is read-only TypeError error
````


Constants are not a restriction on the value itself, but on the variable's assignment of that value. In other words, the value is not frozen or immutable because of `const`, just the assignment of it. If the value is complex, such as an object or array, the contents of the value can still be modified:

````javascript
const a =[];
a.push(1);
console.log(a); //[1]

a = 1; //Type error because the assignment of the variable is tried ot be changed
````

The a variable doesn't actually hold a constant array; rather, it holds a constant reference to the array. The array itself is freely mutable, hence we can easily changes the value within array/object.

**Using `const` in `for loops`**

A `const` can be used in `for..in` and `for..of` loops, because for every loop a new scope is created and within the scope a constant variable is defined, which is not governed by any outer scoped variable, which we occurs when we use `const` in the general `for loop`

````javascript
for (const x in obj) {
  // ..
}

//think of it like this: each loop iteration redeclares, like:

const x = obj[ .. ]
````

****

**Previous Chapter : [`let` declaration](https://github.com/anirudh-modi/JS-essentials/blob/master/Variable-and-scoping/let.md)**


**Next Chapter : [Temporal Dead Zone](https://github.com/anirudh-modi/JS-essentials/blob/master/Variable-and-scoping/Temporal%20dead%20zone.md)**
