## Arrow functions

With ES6, apart from the regular function expression came a new function expression, which can be used to decalre a function, known as the arrow function expression. Apart, from the syntax being different,, an arrow function behaves pretty differently as compared to the traditional functions :

1. The `this` is lexically binded, which means they are always binded to the closest context they are declared in.
2. These are always anonymous functions ie., they can never have a name.
3. They don't have the [[Construct]] function, hence, they cannot be treated as constructor function, and these functions cannot be called using the `new` operator.
4. They do not have any prototype property.
5. Arrow functions doesnot provide with an `arguments` object of its own, so which means to access extra values passed to the arrow function, we need to rely on the `rest` parameters.
6. Their cant be any duplicate name in the parameters, during the declaration of the arrow function.
7. The value of `this` cannot be changed, during the execution of the arrow function or within the code.
