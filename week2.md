# Class 3 Notes

## Objects

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