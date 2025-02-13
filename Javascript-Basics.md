# Class One

## Javascript Basics

Javascript
- most widely used lanuage in the world
- One of the few lanuages preinstalled on most computers
- Interpreted or Compiled Lanuage?
    - Compiled Code (C, C#, Rust, ...):
        - Source code --> Compiler --> Machine Code (executable) --> Execute program
        - Takes your source code and compiles it which then gives you machine code, your executable. Once done you can run the executable
        - Pros: looks for optimizations in your code to make it run faster (removes code that slows you down)
        - Cons: compiling takes a longer time, but the execution is fast!
    - Interpreted Language (Java, Python, Ruby):
        - Source code --> Interpreter (runs line by line) --> Output
        - Takes your source code and interprets it line by line, outputting accordingly
        - Pros: gets an output very quickly
        - Cons: need to install the interpreter, needs to process code line by line!
    - Javascript is both Interpreted AND Compiled as it is interpreted or compiled based on how long each method would take!
        - Creates a copy of your source code and feeds one copy into an interpreter, and one copy into a compiler
        - If Interpreted takes longer than Compiled, than it uses Compiled and the other way around as well!

# Class Two
## Syntax
Printing:
- to print you use `console.log()`
    - whatever is inside the parentheses will be printed

Data types:
- Primitive: numbers, strings, booleans, undefined, null
    - a number is technically a double since its 64 bits
- If you use the `typeof` operator, you can know the data type 

Strings: 
- Unlike Java and other languages, you can use single ('') or double ("") quotes to represent a string
    - Example:
        - `'this is a string'`
        - `"this is also a string"`
- To put a value within a string, you can use ${}, must use (``)
    - Example: 
        ```javascript
            let x = 10
            let s = `the number is ${x}`
        ```

Variable Declaration:
- You can use let and const to declare variables
    - let allows the value of the variable to be changed
    - const disallows the value of the variable to be changed

Objects:
- Javascript is an object oriented lanuage, allowing you to create objects!
- Example:
    ```javascript
        let cat = {name: "Oliver", age: 7}
    ```
    - This is an example of an object
    - With this object you can use the dot operator to access its values!
    - `cat.name // Oliver`
- Everything is an object

Functions:
- Functions are not treated any different than other data types
- Useful Functions:
    - .tostring()
        - converts value to a string
        ``` javascript
            let x = 123
            console.log(x.toString())

            //cannot do
            123.toString()

            // can do
            123..toString()
        ```
    - number()
        - converts input to a number
- Function Declaration
- Functions can be declared out of order since function declarations are hoisted
    - If you save a function to a variable, it won't be hoisted
```javascript
// Examples 
function sum (a, b){
    return a + b;
}
console.log(sum(3, 5)) // 8

// arrow functions, only use if the code is one line (avoid)
const sum = (a, b) => {
    return a + b;
}
```

## Objects Continued
- similar structure of a hashmap
- key/value pairs
- Example:
```javascript
    let myfrac = {num: 3, den: 4, toDecimal: function(){return this.num/this.den}}
    console.log(myfrac.num) // 3
    console.log(myfrac.den) // 4
```
- Example of Deconstructing:
```javascript
    let download_from_web = {a:5, b:7, c:8, z:89}
    let {b, z} = download_from_web
    console.log(b) // 7
    console.log(z) // 89
```

## Spread Operator ...
- Definition: Converts from container type to parameter list
- Example:
    ```js
        let arr1 = [1, 2, 3, 4, 5]
        let arr2 = [8, 9, 10]
        let arr3 = [...arr1, 6, 7, ...arr2, 11, 12, 13]
        console.log(arr3)
        // [1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13]
    ```
- Another Example:
    ```js
    let downloaded = {a: 5, b:7, c:8, z:89}
    let augmented = {...downloaded, d: 100}
    console.log(augmented) // {a:5, b:7, c:8, z:89, d:100}
    ```

## Rest Operator ...
- Converts from parameter list into array
- Example:
    ```js
        let downloaded = [1, 2, 3, 4, 5, 6, 7, 8]
        let [first, ...r] = downloaded
        console.log(first) // 1
        console.log(r) // [2, 3, 4, 5, 6, 7, 8]

## Max Function
- Returns the maximum value of a list
- Example:
    ```js
        console.log(Math.max(1, 2, 3, 4, 5)) //5
        let downloaded = [1, 2, 3, 4, 5]
        console.log(Math.max(...downloaded)) //5
    ```
    - Notice how we used the spread operation in order to pass the paramaters, and did not just pass the list!
- Function Example with Spread:
    ```js
        function myMax(...l){
            let x = l[0]
            for(let i = 1; i < l.length; i++){
                if(l[i] > x){
                    x = l[i]
                }
            }
            return x
        }
    ```