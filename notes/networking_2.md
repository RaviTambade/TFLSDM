# Transflower Mentor Session – 2

## From Networking Fundamentals to Socket Programming: How Applications Talk to Each Other

ENGINEERING FUNDAMENTALS · PYTHON · TCP/IP · CLIENT-SERVER ARCHITECTURE

Today's session: Understanding how two applications communicate using sockets.

## Good morning, everyone!

👨‍🏫 Mentor: Yesterday, we learned about IP addresses and ports. We understood that every computer connected to a network needs an address and that different applications can communicate through different ports. Today, I want to ask you a question. Suppose I have two computers.

- Computer A is running a Python application.
- Computer B is running another Python application.

I want Computer A to send a message: `Hello Transflower!`

Computer B should receive that message and respond: `Welcome Student!`

How will these two applications communicate?

👨‍🎓 Student: Sir, using an API?

👨‍🏫 Mentor: Good answer! But let's go one level deeper. Before REST APIs, before ASP.NET Core controllers, before FastAPI routes, before Express.js endpoints, how does the actual communication happen?

The answer is: Socket Programming.

Today, we will understand the foundation on which network-based applications are built.

## 1. What is a Socket?

Imagine two people talking over a telephone. What do they need?

- A telephone on one side.
- A telephone on the other side.
- A communication connection.
- A way to send and receive information.

A socket provides a communication endpoint that applications use to exchange data. In networking, a socket is associated with communication properties such as:

- IP address
- Port number
- Transport protocol (TCP or UDP)

Think about it:

```
       Computer A                   Computer B
   +----------------+           +----------------+
   | Python App     |           | Python App     |
   |                |           |                |
   | Client Socket  |           | Server Socket  |
   +-------+--------+           +--------+-------+
           |                             |
           |                             |
           +--------- NETWORK -----------+
                    TCP/IP
```

A socket is the programming interface between your application and the network communication facilities provided by the operating system.

Mentor's observation:

An API is a way to expose application functionality. A socket is a lower-level communication mechanism that can carry the data used by that API.

## 2. Client-Server Architecture

👨‍🏫 Mentor: Let's take an example. You visit a restaurant. Who initiates the order?

👨‍🎓 Student: The customer, Sir.

👨‍🏫 Mentor: Correct! Who receives the order?

👨‍🎓 Student: The waiter or restaurant staff.

👨‍🏫 Mentor: Exactly!

In networking:

- Client initiates communication.
- Server waits for incoming connections.
- Client sends requests.
- Server receives and responds.

```
             CLIENT-SERVER MODEL

        CLIENT                    SERVER
   +---------------+         +---------------+
   |               |         |               |
   | Client App    |         | Server App    |
   |               |         |               |
   +-------+-------+         +-------+-------+
           |                         |
           | 1. Connect               |
           |------------------------>|
           |                         |
           | 2. Send Message         |
           |------------------------>|
           |                         |
           | 3. Receive Response     |
           |<------------------------|
           |                         |
           +                         +
```

Examples from real development:

| Client             | Server               |
| ------------------ | -------------------- |
| React application  | Node.js API          |
| Mobile application | ASP.NET Core API     |
| Python script      | Python socket server |
| Browser            | Web server           |
| Database client    | MySQL server         |

## 3. TCP – Transmission Control Protocol

Now let's understand the protocol used for reliable communication. TCP stands for Transmission Control Protocol. Think about sending a registered parcel.

You expect:

1. A connection or delivery arrangement.
2. Data to be delivered reliably.
3. Missing data to be retransmitted where necessary.
4. Data to be reassembled in the correct order.

TCP provides reliable, ordered byte-stream communication. Its important characteristics include:

- Connection-oriented communication.
- Reliable delivery.
- Ordered byte stream.
- Error detection.
- Flow control.
- Congestion control.

### TCP Three-Way Handshake

👨‍🏫 Mentor:

Before two applications communicate through TCP, what needs to happen?

They establish a connection.

This normally begins with a three-way handshake.

```
       CLIENT                         SERVER
          |                              |
          | -------- SYN --------------> |
          |                              |
          | <------ SYN + ACK ---------- |
          |                              |
          | -------- ACK --------------> |
          |                              |
          |                         CONNECTED
```

Explanation:

| Step    | Meaning                          |
| ------- | -------------------------------- |
| SYN     | Client requests connection       |
| SYN-ACK | Server acknowledges and responds |
| ACK     | Client acknowledges server       |

Now TCP connection is established.

Important: TCP provides a byte stream, not a message-based protocol. This distinction will become important when we write our first socket application.

## 4. Socket Programming Lifecycle

Let's understand the responsibilities of a TCP server.

```
           TCP SERVER LIFECYCLE

             socket()
                |
                v
              bind()
                |
                v
             listen()
                |
                v
             accept()
                |
                v
             recv()
                |
                v
             process()
                |
                v
             send()
                |
                v
             close()
```

What does each function do?

| Function   | Responsibility                     |
| ---------- | ---------------------------------- |
| `socket()` | Creates a socket                   |
| `bind()`   | Associates socket with IP and port |
| `listen()` | Marks TCP socket as passive        |
| `accept()` | Accepts an incoming connection     |
| `recv()`   | Receives bytes                     |
| `send()`   | Sends bytes                        |
| `close()`  | Releases the socket                |

The client lifecycle is simpler:

```
          TCP CLIENT LIFECYCLE

              socket()
                 |
                 v
              connect()
                 |
                 v
               send()
                 |
                 v
               recv()
                 |
                 v
               close()
```

Let's implement this ourselves instead of using a framework.

## 5. First Hands-on Lab – Build Your First TCP Server

Live Coding – Python

👨‍🏫 Mentor: Let's create our first network application.

Our objective: A Python server should listen on port `5000` and receive messages from clients.

Step 1: Create `server.py`

```
import socketserver = socket.socket(    socket.AF_INET,    socket.SOCK_STREAM)server.bind(("0.0.0.0", 5000))server.listen(5)print("Transflower Server Started...")print("Waiting for clients...")while True:    client_socket, client_address = server.accept()    print("Connected:", client_address)    message = client_socket.recv(1024)    print("Received:", message.decode())    response = "Welcome to Transflower!"    client_socket.send(response.encode())    client_socket.close()
```

Let's understand the code.

```
socket.socket(    socket.AF_INET,    socket.SOCK_STREAM)
```

Two important parameters:

| Parameter     | Meaning           |
| ------------- | ----------------- |
| `AF_INET`     | IPv4 addressing   |
| `SOCK_STREAM` | TCP communication |

We are creating a TCP socket.

```
server.bind(("0.0.0.0", 5000))
```

The server binds to port 5000 on all available IPv4 interfaces.

```
server.listen(5)
```

The server starts listening for incoming connections. The value `5` is the requested connection backlog, not a limit of five total clients.

```
client_socket, client_address = server.accept()
```

This is interesting! The server waits until a client connects. `accept()` returns a new socket for communicating with that client.

## 6. Build the Client Application

Now create another file.

Step 2: Create `client.py`

```
import socketclient = socket.socket(    socket.AF_INET,    socket.SOCK_STREAM)client.connect(("127.0.0.1", 5000))message = "Hello Transflower!"client.send(message.encode())response = client.recv(1024)print("Server Response:", response.decode())client.close()
```

### Run the applications

Open two terminals.

Terminal 1:

```
python server.py
```

Terminal 2:

```
python client.py
```

Expected output:

Server terminal

```
Transflower Server Started...
Waiting for clients...
Connected: ('127.0.0.1', 52341)
Received: Hello Transflower!
```

Client terminal

```
Server Response: Welcome to Transflower!
```

TCP communication successful Congratulations! You have built a client-server application without using:

- Flask
- FastAPI
- Django
- Express.js
- ASP.NET Core

Only Python's socket library. This is the foundation of network programming.

## 7. What are Bytes? Why Do We Encode and Decode?

👨‍🏫 Mentor: One important question. Why did we write:

```
message.encode()
```

instead of directly sending a string?

👨‍🎓 Student: Because sockets send bytes, Sir?

👨‍🏫 Mentor: Absolutely!  A network communication interface transfers data as bytes. Our Python string:

```
message = "Hello"
```

Must be converted into bytes.

```
data = message.encode("utf-8")
```

Example:

```
String
  |
  | encode()
  v
Bytes
  |
  | Network
  v
Bytes
  |
  | decode()
  v
String
```

| Operation  | Purpose         |
| ---------- | --------------- |
| `encode()` | String to bytes |
| `decode()` | Bytes to string |

This is an important concept because JSON, HTTP, database protocols and messaging systems ultimately exchange encoded data.

## 8. Important Engineering Concept – TCP Message Framing

👨‍🏫 Mentor:

Now let me ask you something interesting. Suppose the client sends:

```
Hello
Welcome
Thank You
```

Will the server always receive exactly three messages?

👨‍🎓 Student: Yes, Sir. Because I sent three messages?

👨‍🏫 Mentor: Not necessarily! This is a common beginner misunderstanding. TCP is a byte stream. It does not preserve application-level message boundaries. One send may be received in multiple reads, or multiple sends may be received together. This is called the TCP Message Framing Problem.
 
For example:

```
SENDER:

send("HELLO")
send("WORLD")

       |
       v

TCP BYTE STREAM:

HELLOWORLD

       |
       v

RECEIVER:

recv() might return "HELLOWORLD"
```

How do engineers solve this?

Common approaches:

1. Fixed-length messages.
2. Delimiter-based framing.
3. Length-prefixed messages.

For our next lab, let's use newline-based framing.

Example:

```
HELLO\n
WORLD\n
THANK YOU\n
```

The receiver reads until it encounters a newline. This is one reason protocols such as HTTP define message framing rules.

## 9. Multi-Client Server – Handling Multiple Users

👨‍🏫 Mentor: Our current server handles one client at a time. Suppose 100 students connect simultaneously. What happens?

👨‍🎓 Student: Sir, the server may have to wait for one client before accepting the next.

👨‍🏫 Mentor: Correct! We need concurrency. One approach is multithreading.

### Threaded TCP Server

```
import socketimport threadingdef handle_client(client, address):    print("Connected:", address)    with client:        while True:            data = client.recv(1024)            if not data:                break            message = data.decode()            print("Received:", message)            response = f"Server received: {message}"            client.sendall(response.encode())server = socket.socket(    socket.AF_INET,    socket.SOCK_STREAM)server.setsockopt(    socket.SOL_SOCKET,    socket.SO_REUSEADDR,    1)
```

Now each accepted client is handled by a separate worker thread.

This is a teaching example. Production servers also need connection limits, timeouts, error handling and resource management.

## 10. TCP vs UDP – Two Ways of Communication

👨‍🏫 Mentor: Imagine two communication scenarios.

Scenario A: You are transferring an important financial transaction. You want reliable, ordered delivery.

Scenario B: You are participating in a live video call. You may prefer timely delivery over retransmitting old information. Can both scenarios use the same transport protocol?

👨‍🎓 Student: No, Sir. TCP and UDP have different characteristics.

👨‍🏫 Mentor: Exactly! TCP  Reliable, ordered byte stream with connection establishment.

UDP

Connectionless datagrams without built-in delivery guarantees.

| Feature        | TCP                                 | UDP                          |
| -------------- | ----------------------------------- | ---------------------------- |
| Connection     | Connection-oriented                 | Connectionless               |
| Reliability    | Built-in                            | Not guaranteed               |
| Ordering       | Preserved                           | Not guaranteed               |
| Retransmission | Supported                           | Not built-in                 |
| Data model     | Byte stream                         | Datagrams                    |
| Common uses    | HTTP/1.1, database connections, SSH | DNS, real-time media, gaming |

Important: UDP applications can implement their own reliability mechanisms when required. QUIC, for example, builds reliable streams over UDP.

## 11. Connect Socket Programming to Fullstack Development

Now let's connect our learning to the technologies you already know.

```
                 FRONTEND
              React / Angular
                     |
                     |
                  HTTP
                     |
                     v
              BACKEND API
           ASP.NET / Node / Python
                     |
                     |
               TCP SOCKET
                     |
                     v
               OPERATING SYSTEM
                     |
                     |
                  NETWORK
                     |
                     v
               DATABASE SERVER
                 MySQL
```

Think about this:

When you execute:

```
fetch("http://localhost:5000/api/students");
```

Your browser does not directly call a Python function. It performs network communication using HTTP over a transport connection (typically TCP for HTTP/1.1 and HTTP/2, or QUIC over UDP for HTTP/3). The server receives the request, processes it and sends a response. This is how networking fundamentals connect to web development.

## 12. Transflower Practical Assignment

Student Challenge

Build a small communication system called TFL Student Communication Server.

### Problem Statement

Create a Python TCP server that allows multiple students to connect and exchange messages.

Functional requirements:

1. Server should listen on port 5000.
2. Multiple clients should connect.
3. Every client should provide a student name.
4. Server should respond with a personalized welcome message.
5. Server should support multiple messages.
6. Client should be able to disconnect using `exit`.

### Suggested project structure

```
TFLCommunication/
│
├── server/
│   ├── server.py
│   └── client_handler.py
│
├── client/
│   └── client.py
│
└── README.md
```

### Evaluation rubric

| Assessment criteria               | Marks |
| --------------------------------- | ----- |
| Socket creation and configuration | 10    |
| TCP connection handling           | 15    |
| Sending and receiving bytes       | 15    |
| Message framing                   | 15    |
| Multiple client handling          | 20    |
| Exception handling and cleanup    | 10    |
| Documentation and demonstration   | 15    |
| Total                             | 100   |

## 13. What Comes Next in Our Engineering Journey?

Our networking learning sequence should progress like this:

1. Networking Fundamentals

   Completed

   IP, ports, DNS, gateway, firewall.
2. Socket Programming

   Today's lesson

   Client-server, TCP sockets, bytes and message framing.
3. TCP vs UDP Deep Dive

   Connection lifecycle, reliability, datagrams and practical labs.
4. Concurrency and Asynchronous Networking

   Sequential processing, threading, asyncio and scalable servers.
5. HTTP Protocol Internals

   Request, response, headers, methods, status codes, TLS and REST.
6. Build a Mini Web Server

   Implement a basic HTTP server using Python sockets.

## Mentor's Closing Thought

Dear students, Today we wrote a program that communicated with another program without any web framework. Think about what we discovered.

A REST API is not magic.

A database connection is not magic.

A web server is not magic.

Underneath these abstractions are operating-system services, sockets, protocols and network communication. 

Frameworks simplify engineering. They should never replace engineering understanding.

Learn the fundamentals first. Then understand the abstractions. Finally, build production systems.

That is