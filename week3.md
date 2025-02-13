# Class 5 Notes

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

# Asynchronous Programming
Blocking:
```js
    x = download() // takes a long time

    Use()

    //User is blocked until the code above is completed, which can take a long time!
```

Threaded Model:
- A programmer had a problem, so he used threads. Then he made more problems

Asynchronous Model:
- Works as follows:
    - I dont know how long this takes but when it finishes, give v8 (Javascript Engine) this callback function to run
