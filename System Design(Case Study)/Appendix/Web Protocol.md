# Two models for networked computer systems

The easiest way to think of the following networking models is as a stack of layers (hence the term "tech stack."). Generally, lower layers package data according to certain protocols which transmit upward where it is received as input by the next layer. This happens again and again until the data reaches its intended destination.

Data is transferred first through physical means like ethernet, then between computers through IP networks, and finally from user to user via the internet over the top, or "application" layer.

There are two models to be aware of. The TCP/IP model which maps more directly to protocols, and the more conceptual 7-layer OSI model.

## TCP/IP Model

The TCP/IP model, named after its two main protocols which we will describe in more detail below, has four layers, starting with the base:

- **The Network Access Layer or Link Layer**, which represents a local network of machines; the "hardware" layer.
- **The Internet Layer**, which describes the much larger network of devices interconnected by IP addresses according to IP protocols (IPv4 or IPv6.)
- **The Transport Layer** which includes protocols for sending and receiving data via packets, e.g. TCP and UDP.
- **The Application Layer**, which describes how data is sent to and from users over the internet, e.g. HTTP and HTTPS.
## OSI Model

The OSI Model or Open Systems Interface model is useful at a conceptual level. It's protocol-agnostic and more detailed. Instead of 4 layers, it breaks the Link Layer and Application Layer of the TCP/IP model into a few more pieces.
## TCP (Transmission Control Protocol) and IP (Internet Protocol)

TCP/IP is the most commonly used protocol suite used today. It includes both the IP (via the internet layer) and TCP (via the transport layer) protocols. How does it work? Data packets are transmitted across network nodes. The Internet Layer connects these nodes through **IP addresses** and TCP protocol operating the transport Layer provides flow-control, establishes connections, and reliable transmission. TCP is a necessary "intermediary" because data packets sent over IP are often lost, dropped, or arrive out-of-order. TCP protocol was built with an emphasis on accuracy, so it's best used in applications where accuracy is more important than speed of delivery.

> **IPv4 vs. IPv6:** You may have come across two different IP protocols — version 4 and version 6. IPv4 addresses include 4 numbers from 0 to 255 separated by periods. This standard was created in the 1980s when the number of available addresses, over 4 billion, seemed like plenty. Obviously, this isn't the case today. IPv6 allows for effectively unlimited IP addresses and includes useful new features (like the ability to stay connected to multiple networks at the same time) and better security, but the two protocols coexist today.

## UDP (User Datagram Protocol)

UDP is a simpler alternative to TCP that also works with the IP protocol to transmit data. It's connectionless, making it much faster than TCP, but because it has none of the error-handling capabilities of TCP, it's error-prone. UDP is mainly used for streaming applications such as Skype, where users accept occasional delays in exchange for real-time service.

Together, TCP and UDP make up most internet traffic at the Transport Layer.

## TCP vs. UDP

TCP emphasizes accurate delivery rather than speed and enforces the "rules of the road", similar to a traffic cop. How? It's **connection-oriented**, which means that the server must be "listening" for connection requests from clients, and the client and server must be connected before any data is sent. Because it's a **stateful** protocol, context is embedded into the **TCP segment** ("packaged" segments of the data stream including a TCP header), meaning that TCP can detect errors (either lost or out-of-order data packets) and request re-transmission.

## HTTP and HTTPS

HTTP (Hypertext Transport Protocol) is the original **request-response** application layer protocol designed to connect web traffic through hyperlinks. It's the main protocol used by everything connected to the Internet. HTTP defines:

- A set of **request methods** (GET, POST, PUT, etc. - the same methods RESTful APIs use)    
- Addresses (known as **URLs**)    
- Default TCP/IP **ports** (port 80 for HTTP, port 443 for HTTPS).    

Every time you visit a site with a http:// link, your browser makes a HTTP GET request for that URL.

HTTP is still in use, but it's been largely replaced by HTTPS (Hypertext Transport Protocol Secure), which serves the same purpose but with much better security features. In 2014, Google announced that it would give HTTPS sites a bump in rankings. That, combined with the increasing need for encrypted data transmission, resulted in much of the web over migrating to HTTPS.

## TLS Handshake Procedure

HTTPS works on top of **TLS (Transport Layer Security)** by default. TLS is a protocol used to encrypt communications in the transport layer, preventing unauthorized parties from listening in on communications. The process for initiating a secure session through TLS is called a TLS handshake. Here's what happens.

- The client requests to establish a secure connection with a server, usually by using port 443 which is reserved for TLS connections.    
- The client and server agree to use a particular cipher suite (ciphers and hash functions.)   
- The server submits a digital certificate which serves as proof of identity. Digital certificates are issued by 3rd party **Certificate Authorities (CAs)** and effectively vouch for the server.
- If the certificate is accepted by the client, the client will generate a session key which is used to encrypt any information transmitted during the session.
Once the session key is created, the handshake is finished and the session begins. All data transmitted will now be encrypted.

## WebSocket vs HTTP

WebSocket is a newer communications protocol designed as an alternative which helps solve some key issues. HTTP was designed to be strictly unidirectional; the client must always request data from the server, and only one HTTP request can be sent per session. Lots of modern applications require longer session times and/or continuous updates from the server. **Long-polling**, a technique that keeps client-server connections open longer, helps, but it doesn't solve the problem — and it's very resource-intensive.

The WebSocket protocol works similarly to HTTP, but with some improvements (and tradeoffs.) It allows servers to send data to clients in a standardized way _without first receiving a request_, and it allows for multiple messages to be passed back and forth over the _same connection_. It's fully compatible with HTTP (and HTTPS), and it's much less computationally demanding than polling.

There are some drawbacks to WebSocket as compared to HTTP, namely:

- WebSocket has no built-in, standardized API semantics like HTTP's status codes or request methods. 
- Keeping communications open between each client and server is more resource-intensive and adds complexity.
- It's less widespread, so development can take longer.

Most WebSocket use cases require real-time data. In a system design interview, consider WebSocket vs. HTTP for applications where updates are frequent, and up-to-date information is critical. Think messaging services, gaming, and trading platforms.

# When to bring this up in an interview

Web protocols might not be an area you're likely to deep-dive or discuss explicitly in your interview, but having a thorough understanding of the architecture underpinning the internet will be extremely helpful in designing anything built to transmit data over the web. If you are asked specific follow-ups, remember:

- At the transport layer, you're likely to choose either the TCP or UDP protocol to send data. Choose TCP if you're more concerned with data accuracy, and UDP if quick transmission is needed (with tolerance for some errors - like in a video streaming application.)
- At the application layer, you have some choices to make as well. You'll probably choose HTTPS over HTTP for security reasons. If you need to maintain open client-server communications (for example, if you're building a fast-paced two-player game and you need to maintain up-to-date scores) you may choose WebSocket over HTTP.
- If you're designing a service with an API, consider HTTP (HTTPS) over WebSocket as you'll be able to make use of HTTPs standardized request methods and status codes; important if you're designing a RESTful API.

## Further reading

- Check out [this article](https://slack.engineering/migrating-millions-of-concurrent-websockets-to-envoy/) on Slack's engineering blog to learn about how Websockets works at scale within Slack.
- [Google's security blog](https://security.googleblog.com/) has covered the migration from HTTP to HTTPS migration from the beginning.