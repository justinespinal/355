# Topic 7

## Asynchronous Pattern
- Demand Driven
- Singular in nature
- Callbacks are required
    - Expectation that the callback will eventually run

## Observer Pattern
- Request Driven Pattern
- Handle 0 or more processes
- You can setup a listener, but that listener is not guaranteed to fire

## Key Features of Observer Pattern
- Subject <=> Observer
    - Each subject can have many observers
    - Observer can't have many subject
- Defining method of a subject is the emit method
    - `emit('eventName'<string>, [...data]);`
- Defining method of an observer is the on method
    - `.on('eventName'<string>, listener<function>)`
    - listener function receives any data passed to emit
- Observer cannot exist without subject (tightly coupled)

Example
```js
// index.js

bambie = new Deer();
//photographer observer
bambie.on("jump", (height) => take_photo(height));
--------------------------------------------------
class{
    ...


    behvaior(){
        if(....){
            this.emit("jump", 20);
        }
    }
}


// Deer.js

```

Another example
```js
const readline = require('readline');
const r1 = readline.createInterface({input: process.stdin, output: process.stdout});

//both are the same
r1.on("line", (input), => console.log(`received ${123}`))
r1.on("SIGSTOP", () => console.log("done with ctrl-z"))
// function ask(){
//     r1.question("Enter some inout: ", (input) => {
//         console.log(`Received: ${input}`)
//         ask();
//     })
// }

// ask();

```

```js

```