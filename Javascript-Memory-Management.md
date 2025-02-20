# Javascript Memory Management
Call Stack and Heap
- Call stack: Stores fixed-size primitives (e.g. numbers, strings). Variables in the call stack include an identifier, address, and value.
- Heap: Stores objects (dynamic types) such as functions and arrays. When a variable stores an object, the object itself is stored in the heap, and the call stack holds a reference to it (a pointer)

Shallow Copy:
- Definition: Values of elements are copied, but nested elements addresses are copied and not their values!

Deep Copy (Nested Copy):
- If using server side applications, you can use the built in method `structuredClone()`
- Example:
    ```js
        let f1 = {num: 3, den: 4, inverse: {num:4, den: 3}}
        let f2 = structuredClone(f1)
        //this will make any changes to f2 not effect f1, and vice-versa
    ```
- Another example:
    ```js
        let f1 = {num: 3, den: 4, inverse: {num:4, den: 3}}
        let f2 = JSON.parse(JSON.stringify(f1))
        //this will make any changes to f2 not effect f1, and vice-versa
    ```
        - this method works for all versions of outdated users, but sometimes may not work for complex data types

## Scope
Global Scope:
- Use case:
    - building tools to help other developers / a library
- Example:
    - `Math.random()`
- Should rarely use global variables
- Declaring a variable as global
    ```js
    function myscope(){
        x = 5; // dont put let/const
        console.log(x)
    }
    myscope()
    console.log(x)
    ```

Function Scope:
- Accessible anywhere in the function, even when not defined
- Example:
    ```js
    function myscope(){
        var x = 5; // put var
        console.log(x)
    }
    ```
- Why it is bad:
    ```js
    function myscope(){
        if(false)
            var x = 5;
        console.log(x) //prints undefined rather than an error
    }
    ```
- Not really useful, has some use cases when you are using a variable that should make sense throughout the entire function. should avoid using

Block Scope:
- Recommended to use
- use let/const
    - `let x = 10`
    - `const y = "hello"`
        - cannot change value of const after declaration
```js
function myscope(){
    if(true){
        let x = 10
        console.log(x) // 10
    }
    console.log(x) // error! x does not exist outside of the conditional statement
}
```

Connecting it all together:
```js
function a(){
    let data1 = 1;
    function b(){
        let data2 = 2;
        function c(){
            let data3 = 3;
            console.log(data1, data2, data3); //will work
        }
        c();
        console.log(data1, data2, data3); //wont work
    }
    b();
    console.log(data1, data2, data3); // wont work
}
```

## Higher Order Functions and Closures
- A higher order function is either:
    1. A function that takes another function as input `and/or`
    2. A function that returns a function as output
- Example:
    ```js
    function create_adder(x){
        return function(y){
            return x + y;
        }
    }

    const add7 = create_adder(7);
    console.log(add7(3)) // 10
    ```
- Another example:
    ```js
    function outer(a){
        let b = 20;
        let unused = 50;
        return function inner(c){
            let d = 40;
            return `${a}, ${b}, ${c}, ${d}`;
        }
    }
    let i = outer(10);
    let j = i(30);
    console.log(j); // 10, 20, 30, 40
    ```
    - with this example if you were to reassign outer to lets say 5 `outer = 5`, then you can no longer call outer `outer(y) error`

## Closure
- A preservation of access to variables that would otherwise be out of scope because of a function dependency

## Not Function Example
```javascript
function not(boolean_func){
    return function(...x){
        return !boolean_func(...x)
    }
}

let is_even = x => x % 2 == 0
let is_odd = not(is_even)
```
Incorrect Way:
```javascript
function not(boolean_func){
    return !boolean_func
}
```

# Special Case before Asynchronous Programming

```javascript
// loop A
let i;
for(i = 0; i < 10; i++){
    void setTimeout(() => console.log(i), 3000)
}
// above loop is not possible as the loop runs faster than the timeout, so it would only print the last value in i, that being 9. This is because i is at the same address on each loop rather than the one below

// loop B
for(let i = 0; i < 10; i++){
    void setTimeout(() => console.log(i), 3000)
}
// this loop prints 0 -> 9 correctly! this is because l is redeclared on each loop, so each iteration has access to a different version of i
```