# Class 3 Notes
# Javascript Memory Management
- All primitive data types go on the stack, all objects on the heap

</br>
</br>

# Class 4 Notes
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