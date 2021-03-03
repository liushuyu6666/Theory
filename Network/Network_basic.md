# Network Basic

[TOC]



## start

### LAN and WAN

- `LAN` is local area network, such as in a house.
- `WAN` connects `LAN`s over large geographic distances.



## OSI reference model

### start

Open System Interconnection Reference Model is a reference model mean it is not an actual model, it is the base of other actual models.

### layers

There are seven layers:

- Application: not the user interface itself.
- Presentation: converts or encodes data into standard format so that receiving computer could understand. For example, text can be encoded as <u>ASCII</u> or <u>HTML</u>, and graphics can be encoded as <u>JPEG</u> or <u>TIFF</u>. 
- Session: manage sessions. For example, a web conferencing application has to maintain separate sessions for each user participating in the conference.
- Transport: converts data to a format that can be transmitted over the network.
- Network: takes the segment from Layer 4 and adds a header to it to create a **package**, and then delivering the package to the destination computer. If there are more than one route to the destination computer. the Network Layer chooses the best path for the packet to take. Since the Network Layer treats each packet independently, so it is possible that two packets from the same transmission might take different paths to arrive at the destination computer.
- Data Link: take package from the upper layer and adds another header to form a **frame**.
- Physical

How data flows? In the sending computer, data flows down the stack, each layer encapsulate the data, adding some information. When the data reach the lowest layer, the physical layer, it will be transported through cable or other physical media to the receiving computer. In the receiving computer, data flow up the stack and be decapsulated by each layer.

### TCP/IP model

Layers are:

- Application (merge of Application, Presentation and Session layers in OSI)
- Transport
- Internet (Network layer in OSI)
- Network Access (merge of Data Link and Physical layers in OSI)

### Five-Layer Model

layers:

- Application (merge of Application, Presentation and Session layers in OSI)
- Transport
- Network
- Data Link
- Physical

## Five-Layer model

### start

layers:

- **Application** (merge of Application, Presentation and Session layers in OSI) 
- Transport **(Layer 4)**
- Network **(Layer 3)**
- Data Link  **(Layer 2)**
- Physical **(Layer 1)**

### data flow (sending side)

Assuming that we have a actual data, flows from application layer to the bottom layer - physical layer.

<img src="img/data_flow.png" style="zoom:67%;" />

The final data should looks like this:

<img src="img/data_encryption.png" style="zoom:67%;" />

#### Application Layer

- When you send an email. it will be first processed by Application Layer protocol, SMTP.
- This layer specifies how data encoded, compressed or encrypted and how sessions should be managed. Analog: translate the language of this letter to the language that recipient could understand.
- Other Protocols are **HTTP**, **FTP**, **DNS**, **SMTP**.
  - **HTTP**: for the transfer of information on the Internet and the World Wide Web.
  - **FTP**: transfer data or file from one computer to another through a network.
  - **DNS**: bind Domain Name to IP.
  - **SMTP**: sending email

#### Transport Layer

- (upper layer protocol) Add **destination soft port number**: specifies the **soft port**, which indicates the **Application Layer protocol** should be used to process the data on the receiving computer.
  - **soft port number** is a unique number assigned to each **Application layer protocol**. So, from this number(port) we can know which **Application Layer protocol** the **Application Layer** use.
  - for example. HTTP is for port 80 and DNS for port 53 SMTP for port 25. So if the soft port number is specified as 25, that means the SMTP protocol will be used in the Application Layer.
-  (upper layer protocol) Add **source soft port number**: records the sending application layer protocol so that the receiving Transport Layer needn't to access to the real data to get it.
- (current layer protocol) Choose a **Transport Layer protocol**: UDP and TCP
  - UDP 
    - no need for receipt, so don't know if the receiver received it.
    - fast and for short message.
    - DNS, Online Game, Voice over IP, Streaming video.
  - TCP
    - need for receipt, error checking and recovery procedures.
    - receiving computer tells the sending computer when the data is received.
    - HTTP, SMTP, FTP.
    - cut data from Application Layer into smaller pieces called segments and use sequence numbers to put segments back in the correct order, if one segment doesn't arrive in time, this segment will be resent.

#### Network Layer

- (upper layer protocol) Add **IP protocol number**: records which **Transport Layer protocol** is used. 
  - Each  **Transport Layer protocol** is assigned an unique identifier or IP protocol number. So from this number the **Transport Layer protocol** could be found.
  - UDP: IP protocol is 17; TCP: IP protocol is 6.
  - It is analogous to the **destination soft port number** and **source soft port number** in **Transport Layer**, both of which record the protocol of the upper layer. But the source of the IP protocol number and the destination of the IP protocol number should be the same.
-  Add **source IP address**:
- Add **destination IP address**:

#### Data Link Layer

- (upper layer protocol) Add **Layer 3 Protocol**: records which **Network Layer protocol** the upper layer use.
- Add **destination layer 2 address** or **MAC address**:
- Add **source layer 2 address** or **MAC address**:
- data integrity check: **checksum** at the end.
- convert the data into ones and zeros of digital communications.

#### Physical Layer

- convert bits into electrical signals and send them across the physical medium.

### data flow (receiving side)

#### Physical Layer

- received electrical signal

#### Data Link Layer

- reassembled bits into a frame.
- validate the checksum, if invalid, dismiss it.
- remove Layer 2 headers and tail.

#### Network Layer

- verify if package has reached the correct IP address
- send it to TCP or UDP protocol
- remove Layer 3 headers

#### Transport Layer

- If TCP:
  - collect all the segments.
  - reassemble the data.
  - send acknowledgement to the sending side indicating the data was received. If sending end didn't receive this acknowledgement it will resend the segment.
  - send it to the correct application layer protocol.

#### Application Layer

- process data using the specified Application Layer protocol.

### data unit

Name of data unit at each level

- Transport Layer: Segment
- Network Layer: Packet
- Data Link Layer: Frame



## Routing Model (Layer 3 across the network)

### start

- **MAC address** assigned by the manufacturer is unique. **IP address** is assigned by network administrator.
- **broadcast domains**: Devices in broadcast domain can be reached by broadcast **MAC address**
- broadcast address: `ffff.ffff.ffff`. Every one but the source device in the broadcast domain can receive the message when broadcast address is used.
- In address resolution protocol (ARP), source device use target's IP address to get MAC address. Although IP address is prerequisite for the source device, it still need target's MAC address. This is because in the encapsulation and decapsulation, the layer can't be jump over.
- IP comprises network number and host number, in the same network, the network number should be the same but host number should be unique. While across network the network number is unique but host number could be the same.
- router is a Layer 3 device indicates that it can modify Layer 2 address of the received message
- boarding table is maintained by the **switch**, if it can't find the MAC address, it will use ARP to populate it.

### data flow in different cases

Here we use simplified MAC address and IP address, such as `ab` and `1.2` where `1` means the network domain and `2` means host domain

<img src="img/network.png" style="zoom:150%;" />

#### broadcast

Use broadcast address `ffff.ffff.ffff`. 

1. source device sends the message whose destination is a broadcast address to all devices.
2. when **bridge** or **switch** get it, they will forward it out of every port except the received one.

#### address resolution protocol (ARP)

In this scenario, computer A wants to send message to computer B within the same network.

1. check the IP address to see if in the same broadcast domain. A is 1.1 and B is 1.2, the same prefix indicates that they are in the same domain or network.
2. computer A need to know the MAC address of computer B, so it send a Layer 2 broadcast frame request, asking the MAC address of the device whose IP address is 1.2. Everyone in the network could hear.
   - this Layer 2 broadcast frame request contains:
     - destination Layer 2 address: `ffff.ffff.ffff`. This is a Layer 2 protocol.
     - source Layer 2 address: `4a` This is a Layer 2 protocol.
     - sender's MAC address: `4a`
     - sender's Layer 3 address: `1.1`
     - target Layer 2 address: `0000.0000.0000` indicating it is unknow.
     - target Layer 3 address: `1.2`
3. only computer B will response after checking the target Layer 3 address.
   - this response contains:
     - destination Layer 2 address: `4a` This is a Layer 2 protocol.
     - source Layer 2 address: `ab` This is a Layer 2 protocol.
     - sender's Layer 2 address: `ab`
     - sender's Layer 3 address: `1.2`
     - target Layer 2 address: `4a`
     - target Layer 3 address: `1.1`
4. Then A and B can communicate through MAC address without disturbing others.

#### when router get a package

In this scenario, router E receive a package from port 1.

1. check the MAC address to see if router E is the right recipient. if the destination Layer 2 address in the package is `ac` that means it is right.
2. remove Layer 2 frame such as destination Layer 2 address and source Layer 2 address.
3. inspect Layer 3 information, if the destination IP address is `3.2`, the router E will check the router table and know it needs to be forwarded to next router whose IP address is `2.10` from port 2.
4. add new Layer 2 frame, including destination Layer 2 address (`34`) and source Layer 2 address (`b8`).

#### send data across the network

In this scenario, Computer A send data to Computer C across network.

1. Computer A encapsulate the data from the top of 5-layer model to the bottom.

   - At the Layer 3, source Layer 3 address will be `1.1` and destination will be `3.2`.

   - At the Layer 2, Computer A need to clarify if Computer C is in the same network by check the destination IP address. Obviously they are not in the same network. So it will send the data to its configured default gateway / router. In this case, the destination Layer 2 address is `ac` and the source is `4a`.

     - if Computer A didn't know the configured default gateway's MAC address yet (already known the IP address of its default gateway because it is configured), it will use ARP to get the MAC address.
     - The default gateway of a PC is either configured by the network administrator or learned dynamically.

   - the data:

     | destination layer 2 address | source layer 2 address | protocol  type | source IP address | destination IP address | IP protocol number | original data | checksum |
     | --------------------------- | ---------------------- | -------------- | ----------------- | ---------------------- | ------------------ | ------------- | -------- |
     | ac                          | 4a                     |                | 1.1               | 3.2                    |                    |               |          |

     

2. Switch D will check the boarding table, or MAC address table which binds the MAC address with the port to decide the exit table. if there is no such MAC address, it will use ARP to get populate the table. In this case, the package will send to Router E.

   <img src="img/boarding_table.png" style="zoom:80%;" />

   - Switch D just inspect Layer 2 frame, can't modify Layer 2 frame or check Layer 3 frame.

   - the data:

     | destination layer 2 address | source layer 2 address | protocol  type | source IP address | destination IP address | IP protocol number | original data | checksum |
     | --------------------------- | ---------------------- | -------------- | ----------------- | ---------------------- | ------------------ | ------------- | -------- |
     | ac                          | 4a                     |                | 1.1               | 3.2                    |                    |               |          |

3. what Router E do, see the section "when router get a package". Briefly speaking, remove old Layer 2 address, check the new Layer 2 address and update the Layer 2 address.

   - router can change Layer 2 address and inspect Layer 3 address

   - the data: 

     | destination layer 2 address | source layer 2 address | protocol  type | source IP address | destination IP address | IP protocol number | original data | checksum |
     | --------------------------- | ---------------------- | -------------- | ----------------- | ---------------------- | ------------------ | ------------- | -------- |
     | 34                          | b8                     |                | 1.1               | 3.2                    |                    |               |          |

4. Switch F get the data...

   - the data:

     | destination layer 2 address | source layer 2 address | protocol  type | source IP address | destination IP address | IP protocol number | original data | checksum |
     | --------------------------- | ---------------------- | -------------- | ----------------- | ---------------------- | ------------------ | ------------- | -------- |
     | 34                          | b8                     |                | 1.1               | 3.2                    |                    |               |          |

5. Router G get the data...

   - the data:

     | destination layer 2 address | source layer 2 address | protocol  type | source IP address | destination IP address | IP protocol number | original data | checksum |
     | --------------------------- | ---------------------- | -------------- | ----------------- | ---------------------- | ------------------ | ------------- | -------- |
     | e2                          | 5f                     |                | 1.1               | 3.2                    |                    |               |          |

6. the computer C receive the package, it will decapsulate the package.

   

#### summary

- if a PC find the destination IP address doesn't reside in its network, it will send the package to its default configured gateway.
- if a router doesn't know the IP address, it will send the package to the default path of the router table.
- while sending a package across the network, IP address always records the ultimate destination but the MAC address only points to the next stop and will be updated by Layer 3 devices such like router.
- **router** is a Layer 3 device and **switch** is a Layer 2 device. The former could update the MAC address and inspect IP address, the latter could only inspect the MAC address.