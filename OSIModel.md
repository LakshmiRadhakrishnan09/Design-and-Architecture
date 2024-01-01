7 layers

Application Layer
Presentation Layer
Session Layer
Transport Layer
Network Layer
Data Link Layer
Physical Layer

Application Layer : Browser, SMTP
Presentation Layer : Encryption/Decryption, Compression
Session Layer : Sesion establishment
Transport Layer : End to end delivery of message. 
Network Layer : Host to host delivery. Routing layer
Data Link Layer : Node to node delivery of message. Transporting frames.
Physical Layer : Actual physical connection between devices. Transporting bits.

## TCP/IP Model
Application Layer
Transport Layer(TCP/UDP)
    TCP : Reliable connection(Delivery of data is guarenteed. "hand shake" process.) between sender and receiver. Keeps track of sequence by assigning numbers to every single segment.A connection must be established before transferring data.
    guarantees that the data packets will be delivered in the order they were sent. 
    For transfering images and videos. Browsing, Sending emails.
    TCP is used by HTTP, HTTPs, FTP, SMTP and Telnet.
    
    UDP : Unreliable(There is no ACK on data received.) and connection-less protocol. No need to establish a connection before data transfer. There is no handshake established before sending data.
    Is faster than TCP.
    No sequencing of data.
    Gaming, Video Streaming, Online Video Chats
    UDP is used by DNS, DHCP, TFTP, SNMP, RIP, and VoIP.
    UDP is generally used when speed matters most rather than the quality of the product. Some data packets might be lost or somehow received out of order, but UDP is more prominently used in the case of live streaming.
Network/Internet Layer(IP)
Data Link Layer (MAC)
Physical Layer


## Chat Message Protocol

1. WebRTC ( Web Real Time communication):
        Browser based
        Use UDP
        Created by Google, Open source
2. WebSocket
3. MQTT
4. AMQP

