## Methods in String wrapper

With ES6, a couple of new methods were included in the native `String` wrapper object, which gives the power to search within a string more effectively! Let's look into these methods.

### `String.prototype.startsWith()`
This method checks whether a particular `string` starts with a particular `string` or not, it will return `true` or `false`.

````javascript
var str1 = `Hello, this is me!`;

console.log(str1.startsWith('Anirudh'));   // false
console.log(str1.startsWith('Hello'));     // true
console.log(str1.startsWith(`Hello
    `));                     // false
````

This method also accepts an optional second parameter, which is the specified the position from which the `string` search will begin, the position count starts from `0`, the index specified will also be included while searching. And by defaults the value of position is 0. This methods is also case sensitive.

````javascript
var str1 = `Hello, this is me!`;
console.log(str1.includes('el',1));   // true

console.log(str1.includes('el',2));   // false

console.log(str1.includes('El',1));   // false
````

>Note : The first argument must only be a `string`, this cannot be a `regex`, it throw a `TypeError`.
For further reading:
http://www.ecma-international.org/ecma-262/6.0/#sec-string.prototype.startswith

### `String.prototype.endsWith()`
This method checks whether a particular `string` end with a particular `string` or not, it will return `true` or `false`.

````javascript
var str1 = `Hello, this is me!`;

console.log(str1.endsWith('Anirudh'));   // false
console.log(str1.endsWith('me'));        // false
console.log(str1.endsWith(`e!`));        // true
````

This method also accepts an optional second parameter, which actually specifies the number of characters of the original `string` which must be searched starting from `1st` character, which means if the original string was `Hey!`, and we use `.endsWith('y!',2)`, then the search will not be performed on `Hey!`, but the search will be performed on `He`. And by defaults the value of position is the length of the string. This methods is also case sensitive.

````javascript
var str1 = `Hello, this is me!`;
console.log(str1.endsWith(`e!`,1));   // false because the string being search
                                      // within is only "H"
console.log(str1.endsWith('e',2));   // true
````

>Note : The first argument must only be a `string`, this cannot be a `regex`, it throw a `TypeError`.
For further reading:
http://www.ecma-international.org/ecma-262/6.0/#sec-string.prototype.endswith

### `String.prototype.includes()`

This method basically checks whether a particular `string` exist within another `string` or not, it will return `true` if the `string` is present in another `string` else, it will return `false`. Let's look by example,

````javascript
var str1 = `Hello, this is me!`;

console.log(str1.includes('Anirudh'));   // false
console.log(str1.includes('Hello'));     // true
console.log(str1.includes(`Hello
    `));                     // false
````

This method also accepts an optional second parameter , which is the specifies the position after which the `string` must search must be searched, the position count starts from `0`, the index specified will also be included while searching. And by defaults the value of position is 0. This methods is also case sensitive.

````javascript
var str1 = `Hello, this is me!`;
console.log(str1.includes('l',3));   // true
console.log(str1.includes('l',4));   // false

console.log(str1.includes('HELLO'));   // false
````

>Note : The first argument must only be a `string`, this cannot be a `regex`, it throw a `TypeError`.
For further reading:
http://www.ecma-international.org/ecma-262/6.0/#sec-string.prototype.includes

### `String.protoype.repeat()`
This `repeat` method creates a new `string` and returns that newly created string, it accepts a count parameter which specifies the number of copies a string to be created. The count can be any positive number, less than `infinity`, else it throws a `Range Error`.

````javascript
var str = 'Hello';
str.repeat(2);          // HelloHello
str.repeat(1);          // Hello
str.repeat(0);          // "" Since no repeatation was mentioned
str.repeat(-1);         // RangeError
````

### `String.raw`
`String.raw` is a native tagged template literal added to the `String` wrapper, which can help us get the raw value of a template string, by raw we mean the value of the string will be uncooked, implying that the string `backslashes` within the string will not be escaped. Let's look at an example to understand what it means.

````javascript
var temp = `Hello\nWorld`;

console.log(temp);              // Hello
                                // World
console.log(String.raw`Hello\nWorld`);                        // Hello\nWorld
````

You can see that the `\n`is not escaped in case of the `String.raw`.
