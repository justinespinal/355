# Data Link Layer (DLL)
1. Provide translation from digital bits into physical signal, as well as the reverse
2. Provide hop-to-hop delivery between two directly connected adjacent nodes using a single transmission medium (can be wireless)

Addressing:
- DLL: `MAC Address`
- Network Layer: `IP Address`
- Transport Layer: `Logical Port`

MAC Address:
- a globally unique 48 bit hardware identifier
- assigned to each network card
- central authority manages all MAC Addresses (IEEE)
- Example:
    - 6A-3C-44-0F-ED-12
- First half of the MAC Address is the OUI:
    - Organizational Unique Identifier
    - `6A-34-44`-0F-ED-12
    - Linked to each network card manufacturer
- Second half of the MAC Address if the NIC:
    - Network Interface Component
    - 6A-34-44-`0F-ED-12`
    - Decided by the network card manufacturer

Two Major Protocols Used on the Data Link Layer (DLL)
- Ethernet (Wired Connections / 802.3):
    - Header + Data
- WiFi (Wireless / 802.11):
    - Complex Protocol

Sending a message through ethernet and through wifi
A -(ethernet) B - C (wifi) :

A -> B

**DLL Header (Ethernet)**
- src MAC: A
- dest MAC : B
- **Network Layer**
    - src IP: A
    - dest IP: C
    - message

B -> C

**WiFi Frame**
- src MAC: B
- dest MAC : C
- **Network Layer**
    - src IP: A
    - dest IP: C
    - message

Ethernet:
- First 8 bits are used for synchronization purposes (Preamble)
    - SFD signafies that this is the end of preamble and the start of the message (double 1)
- Next 12 bits store Dest and Src MAC
- EtherType (2 bits) used to define the protocol that is one layer higher
    - tells receipient device how to interpret the payload
    - common values:
        - IPv4 -> 0x0800
        - IPv6 -> 0x86DD
        - ARP  -> 0x0806
        - IPv4 + 802.1Q -> 0x8100
- next n bits is the payload
    - n = 46 -> 1500
- CRC/FCS is 4 bits
    - FCS: Frame Check Sequence
        - provides data integrity (not reliable delivery)
            - ability to detect when your messages are corrupted
    - Internally we use CRC32
        - Cyllic Redundancy Check
        - Variable Sized Input -> CRC32 -> Fixed Length Digest (32 bits)
            - one way
            - once you get digest, you glue it to the end of the header + message
            - then take the entire message and send it out to B
            - when it arrives, B knows the last 32 bits are the digest because we are using ethernet
            - B detaches the last 32 bits, and sends the header + message into CRC32
            - B now has another digest (digest 2), and if they was no corruption, both digests should be the same
            - if the digests are the same, there was no corruption!
            - if the message is corrupted, a re-transmission does not occur! up to a higher layer protocol to decide what to do when corrupted
## Data Link Hardware Device: Network Switch
- Acts as a n-dimensional cable
- Has CPU + memory
- Similar to the hub, but doesn't send message to all devices, only to the intended device
- Passive Device
    - will never send a message out on its own
- Do not have their own MAC Address

Switch Device Discovery Algorithm
1. upon receiving a message, record { src MAC + physical port } into `Forwarding Information Base (FIB)`
2. Check { dest. MAC} if a matching record exists in FIB, forward message out on matching {physical port}. else (if no record exists) FALLBACK TO HUB MODE + BROADCAST MESSAGE TO ALL OTHER PORTS EXCEPT RECEIVING PORT (the device that sent the message)
3. Upon the first message sent by a device, add it to the FIB

Fowarding Information Base is a table containing MAC Address, Physical Port, and Expires date



Wireless (WiFi: 802.11):
- Ethernet is a data + header protocol
- WiFi has a much more complex protocol
    - hundreds of different types of messages
- different types of messages follow into categories:
    - Management Frames
        - Beacon
            - emitted by the router to signal that this network is connectable
        - Probe
            - constantly checking if a known network is nearby
        - Authentication
            - provide password to wifi you are connection to, encrypted
        - Association
            - associating your device with the network, and being able to access the network resources
    - Control Frames
        - Acknowledgement
            - continue to send message until it is confirmed to have been sent
        - Block Acks
            - 
        - RTS + CTS
            - Solves Hidden Node Problem (devices out of range)
    - Data Frames (Data + Header)


------