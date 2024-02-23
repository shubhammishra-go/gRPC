# gRPC

gRPC is a cross-platform open source high performance remote procedure call (`RPC`) `framework`.

it was initially created by Google, which used a single general-purpose RPC infrastructure called `Stubby` to connect the large number of `microservices` running within and across its data centers from about `2001`.

In `March 2015`, Google decided to build the next version of Stubby and make it open source. The result was gRPC, which is now used in many organizations aside from Google to power use cases from microservices to the "last mile" of computing (mobile, web, and Internet of Things).

It uses `HTTP/2` for transport, `Protocol Buffers` as the `interface description language`, and provides features such as authentication, `bidirectional streaming` and flow control, blocking or nonblocking bindings, and cancellation and timeouts. It generates cross-platform client and server bindings for many languages. Most common usage scenarios include connecting services in a microservices style architecture, or connecting mobile device clients to backend services.


![alt text](grpc-icon-color.png)


# About RPC 

Remote Procedure Call is a `distributed computing technique` in which a computer program calls a procedure (subroutine or service) to execute in a different address space than its own. The procedure may be on the same system or a different system connected on a network.

The idea behind RPC is that a computer program can call and execute a subroutine on a remote system just like it would call a local subroutine, but the network communication details are hidden from the user.

RPC is a request–response protocol. it supports `process-oriented` and `thread-oriented` models.

The client makes a request to execute a procedure on the remote server. Like a synchronous local call, the client is suspended until the procedure results are back. The procedure’s parameters are passed over the network to the server-side. The procedure executes on the server and, finally, the results are transferred back to the client.


RPCs are a form of `inter-process communication (IPC)`, in that different processes have different address spaces: if on the same host machine, they have distinct virtual address spaces, even though the physical address space is the same; while if they are on different hosts, the physical address space is different. Many different (often incompatible) technologies have been used to implement the concept. 


![alt text](image.png)


# What does RPC do?

When program statements that use the RPC framework are compiled into an executable program, `a stub is included in the compiled code` that acts as the representative of the `remote procedure code`. When the program is run and the procedure call is issued, the stub receives the request and forwards it to a client runtime program in the local computer. The first time the client stub is invoked, it contacts a name server to determine the transport address where the server resides.

The client runtime program has the knowledge of how to address the remote computer and server application and sends the message across the network that requests the remote procedure. Similarly, the server includes a runtime program and stub that interface with the remote procedure itself. Response-request protocols are returned the same way.


# How does RPC work?

When a remote procedure call is invoked, the calling environment is suspended, the procedure parameters are transferred across the network to the environment where the procedure is to execute, and the procedure is then executed in that environment.

When the procedure finishes, the results are transferred back to the calling environment, where execution resumes as if returning from a regular procedure call.

During an RPC, the following steps take place:

    
1.  The client calls the client stub. The call is a local procedure call with parameters pushed onto the stack in the normal way.

2. The client stub packs the procedure parameters into a message and makes a system call to send the message. The packing of the procedure parameters is called marshalling.

3. The client's local OS sends the message from the client machine to the remote server machine.
    
4. The server OS passes the incoming packets to the server stub.

5. The server stub unpacks the parameters -- called unmarshalling -- from the message.

6. When the server procedure is finished, it returns to the server stub, which marshals the return values into a message. The server stub then hands the message to the transport layer.

7. The transport layer sends the resulting message back to the client transport layer, which hands the message back to the client stub.

8. The client stub unmarshalls the return parameters, and execution returns to the caller.


# About Stub

a stub is a `program` that acts as a temporary replacement for a remote service or object.It allows the client application to access a service as if it were local, while hiding the details of the underlying network communication. This can simplify the development process, as the client application does not need to be aware of the complexities of distributed computing. Instead, it can rely on the stub to handle the remote communication, while providing a familiar interface for the developer to work with. 

The stub represents a `remote object` or `service on a local system`. It acts as a `proxy` for the remote service and allows the client program to make method calls on the remote object as if it were local. The process of generating stubs involves creating a client-side proxy object that provides the same interface as the remote service, but routes method calls to the actual remote object

The main purpose of an RPC is to allow a local computer (client) to invoke procedures on a remote computer (server). Since the client and server have different address spaces, the parameters used in a function call must be converted. Otherwise, the parameter values would be unusable because pointers to parameters in one computer's memory would reference different data on the other computer. Also, the client and server can have different data representations, even for simple parameters like integers (e.g., big-endian versus little-endian). Stubs handle parameter conversion, making a remote procedure call look like a local function call to the remote computer.




# About Protocol buffers

Protocol Buffers (`Protobuf`) is a free and open-source cross-platform data format used to serialize structured data. It is useful in developing programs that communicate with each other over a network or for storing data.
The method involves an `interface description language` that describes the structure of some data and a program that generates source code from that description for generating or parsing a stream of bytes that represents the structured data. 

//About Interface Description Language

An interface description language or interface definition language (IDL) is a generic term for a language that lets a program or object written in one language communicate with another program written in an unknown language. IDLs are usually used to describe data types and interfaces in a language-independent way, for example, between those written in C++ and those written in Java.

IDLs are commonly used in `remote procedure call software`. In these cases the machines at either end of the link may be using different operating systems and computer languages. IDLs offer a bridge between the two different systems. 




# gRPC vs GraphQL

Here we will compare gRPC and GraphQL in terms of some parameters...

`Payload` ::: Message size matters because smaller messages generally take less time to send over the network. gRPC uses `protocol buffers` (a.k.a. protobufs), a `binary format` that just includes values, while GraphQL uses JSON, which is text-based and includes field names in addition to values. The binary format combined with less information sent generally results in gRPC messages being smaller than GraphQL messages. (While an efficient binary format is feasible in GraphQL, it’s rarely used and isn’t supported by most of the libraries and tooling.) 
gRPC’s binary format is faster `serializing` and parsing of messages compared to that of GraphQL’s text messages. The downside is that it’s harder to view and debug than the human-readable JSON.

`Network` ::: gRPC uses `http/2` network protocol. http/2 provides `multiplexing` multiple requests over a single TCP connection (fixing the HTTP-transaction-level head-of-line blocking problem in HTTP 1.x). http/2 also provides many features like data compression etc.. which definatly going to reduce latency. while in GraphQL there is no such features.

`Streaming` ::: Both gRPC and GraphQL support the server streaming data to the client: gRPC has server streaming and GraphQL has Subscriptions and the directives @defer, @stream, and @live. gRPC’s HTTP/2 also supports `client and bidirectional streaming` (although we can’t do bidirectional when one side is a browser). HTTP/2 also has improved performance through multiplexing.
gRPC has `built-in retries` on network failure, whereas in GraphQL, it might be included in your particular client library, like Apollo Client’s RetryLink. gRPC also has built-in deadlines.

`Proxy` ::: gRPC is unable to use most `API proxies` like `Apigee Edge` that operate on HTTP headers, and when the client is a browser, we need to use` gRPC-Web proxy` or `Connect` (while modern browsers do support HTTP/2, there aren’t browser APIs that allow enough control over the requests).

// About Proxy Server

A proxy server is a `server application` that acts as an `intermediary between a client requesting a resource and the server` providing that resource.[1] It improves privacy, security, and performance in the process. 
Instead of connecting directly to a server that can fulfill a request for a resource, such as a file or web page, the client directs the request to the proxy server, which evaluates the request and performs the required network transactions. This serves as a method to simplify or control the complexity of the request, or provide additional benefits such as load balancing, privacy, or security. Proxies were devised to add structure and encapsulation to distributed systems.A proxy server thus functions on behalf of the client when requesting service, potentially masking the true origin of the request to the resource server.


`Schema` ::: in gRPC version 3 it does not have required fields: instead, every field has a default value. In GraphQL, the server can differentiate between a value being present and absent (null), and the schema can indicate that an argument must be present or that a response field will always be present.
there is no standard way to know whether a method will mutate state in gRPC. while in GraphQL, which has separated queries and mutations type. `Maps` are supported in gRPC but not in GraphQL: if you have a data type like `{[key: string] : T}`, you need to use a JSON string type for the whole thing.


`Underfetching` In gRPC, we call methods one at a time. If we need more data than a single method provides, we need to call multiple methods. And if we need response data from the first method in order to know which method to call next, then we’re doing multiple round trips in a row. Unless we’re in the same data center as the server, that causes a significant delay. This issue is called underfetching.

This is one of the issues GraphQL was designed to solve. It’s particularly important over high-latency mobile phone connections to be able to get all the data you need in a single request. In GraphQL, we send a string (called a document) with our request that includes all the methods (called queries and mutations) we want to call and all the nested data we need based on the first-level results. Some of the nested data may require subsequent requests from the server to the database, but they’re usually located in the same data center, which should have sub-millisecond network latency.


# Ref

`https://www.javatpoint.com/what-is-rpc-in-operating-system`

`https://book.systemsapproach.org/e2e/rpc.html`

