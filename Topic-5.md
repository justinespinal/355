# Topic 5: Physical Layer
- Understanding that the physical properties of transmission mediums defines the protocols we use.
- More specifically:
    - Bit by bit encoding of bits into physical signal

## Two Major Categories
Cables
- point to point
    - connects exactly two devices
- do not provide encryption by default
- types:
    - copper (twisted pair):
        - encode information as pulses of electricity
    - fiber
        - fully enclosed glass tubes with a mirror shielding
-   802.3 Ethernet Data Link Layer (DLL)

Wireless
- nondirectional
    - will work anywhere
    - messages emitted go in every single direction
- encryption provided on the protocol level
- 802.11 Protocol Data Link Layer (DLL)

Channel Types:
- Simplex (unidirectional)
- Duplex (bidirectional)
- Full Duplex (Send & Receive at the same time)
- Half Duplex (One receiver at any time)
    - Wireless operates at Half Duplex

Hardware:
- Network Hub (Historical, 1950s) 
    - Solve the limitation of the cable (point to point)
    - Act as an n-dimensional cable
    - Saves money but not good for security
    - No CPU
    - More than one message at a time causes message to fail
- Algorithm to solve the above problem:
    - Carrier Sense Multiple Access (CSMA)
        - First version: if you are receiving electrical current, wait! Someone is using the network.
        - ```js
          function send(message){
             while(receiving){wait}
             send(message)
          }
          ```
        - Problem was that devices that did not use CSMA would corrupt data
    - Upgrade: CSCMA/CD V2
        - Carrier Sense Multiple Access + Collision Detection
            ```js
            function CSMA(message){
                for(i = 0; i < message.length; i++){
                    while(receiving) wait();
                    send(message[i])
                }
            }
            ```
        - Slower than first version, but maximizes throughput
        - Encourages data to flow even if we have to slow down

    - Network Switch Invented 20 years later:
        - have a CPU and memory
            - allows for message queues to solve collision
        - can send messages to individual devices rather than all devices connected to the hub
        - cause CSMA to fall out

    - Third Version of CSMA for Wireless (CSMA/CA):
        - Carrier Sense Multiple Access + collision avoidance
        - Computers would wait the same amount of time on collision, causing more future collisions
        - Insatead would wait a random amount of time to fix!
        ```js
            function CSMA(message){
                for(i = 0; i < message.length; i++){
                    while(receiving) wait(random);
                    send(message[i])
                }
            }
            ```
