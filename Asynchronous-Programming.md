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
    - I dont know how long this takes but when it finishes, give v8 (single-threaded Javascript Engine) this callback function to run

Asynchronous Programming with Node.js/Libuv:
- Javascript delegates asynchronous code to Libuv and continues running code as normal while Libuv handles the asynchronous code
- Application
    - Javascript <-> Node.js Bindings (Node API)
        - Libuv
            - Asynchronous is put onto the Task Queue and wait for a worker thread in the worker thread pool to be available
            - Worker thread starts processing asynchronous code
            - Passes any callback functions to the Event (Callback) Queue through an Event Loop, moved into V8 once ready to execute
                - Event Loop just constantly checks that no code is in v8, makes sure that no code overlaps one another
                - Procedural Code always has priority
- Example:
```javascript
const fs = require("fs")
fs.readFile("/...", "utf8", do_after_reading)
function do_after_reading(err, data){
    if(err){
        console.log(err)
    }else{
        console.log(`Finished reading file: ${data.length}`)
    }
}
for(let i = 0; i< 500000000; i++){
    // intentially slow
}
console.log("hello")

// do_after_reading will end up running AFTER the slow for loop and print statement since v8 has code
```
![image](https://miro.medium.com/v2/resize:fit:1400/format:webp/1*rWGbyCbcJTKI-m3ZEDhCaA.png)

Working with asynchronous programs can be messy, creating functions for any different route code can take, and callback functions not having access to what was passed to libuv. So resolve with closures
```javascript
const dns = require("dns")
const venus = "venus.cs.qc.cuny.edu"
const mars = "mars.cs.qc.cuny.edu"

resolve(venus)
resolve(mars)

function resolve(domain){
    dns.resolve(domain, after_resolve)

    function after_resolve(err, data){
        if(err){
            console.log(err)
        }else{
            console.log(domain, data)
        }
    }
}

// You can also do recursively for concurrent behavior
```
Concurrent Method
```javascript
const dns = require("dns")
const domains = ["apple.com", "microsoft.com", "walmart.com"]
let i = 0
dns.resolve(domains[i], after_resolution)
function after_resolution(err, records){
    console.log(domains[i], records)
    i++
    if(i >= domains.length){
        console.log("all resolved")
    }else{
        dns.resolve(domains[i], after_resolution)
    }
}
```

## How to run in parallel and maintain output order
```js
const dns = require("dns")
const domains = ["apple.com", "microsoft.com", "walmart.com"]
let domains_resolve = 0
let results = Array(domains.length)
for(let i = 0; i < domains.length; i++){
    resolve(domains[i], i)
}

function resolve(domain, i){
    dns.resolve(domain, after_resolution)
    function after_resolution(err, records){
        domains_resolves++
        if(err){
            console.error("Error")
        }else{
            results[i] = `${domain}: ${records.join(",")}`
            if(domains_resolve >= domains.length){
                fs.writeFile("output.txt", results.join("\r\n"), () => console.log("done"))
            }
        }
    }
}
```
