---
layout: post
title:  "Introduction to gRPC"
kicker: "RPC"
subtitle: "gRPC is an open-source, high-performance RPC framework that can run in any environment. gRPC builds on HTTP/2 protocol and the protobuf message-encoding protocol to provide high performance, low-bandwidth communication between applications and services."
image: assets/images/posts-cover-images/grpc.jpg
author: senthil
date: 2022-11-11 00:00:00 +0530
tags: ["rpc", "remote-procedure-call", "grpc", "inter-process-communication"]
categories: rpc
featured: false
hidden: true
toc: true
---

# Overview

**gRPC** is an open-source, high-performance **Remote Procedure Call** (**RPC**) framework that can run in any environment. With pluggable support for *load balancing*, *tracing*, *health checking*, and *authentication*, it can efficiently connect distributed services in and across data centers. gRPC builds on **HTTP/2 protocol** and the **protobuf** (protocol buffers) message-encoding protocol to provide high performance, low-bandwidth communication between applications and services. gRPC can use protocol buffers as both its [**Interface Definition/Description Language**]({{ site.baseurl }}/rpc/2022/introduction-to-grpc#what-is--interface-definition-language-idl) (**IDL**) and as the format for exchanging messages.

It can generate code for both the server and the client in the most common programming languages and platforms, such as.NET, Java, Python, Node.js, Go, and C++. With gRPC, a client application can directly call a method on a server application on a different machine as if it were a local object. This makes it easier to create distributed applications and services.

As with most RPC systems, gRPC is based on the idea of defining a service by specifying the methods that can be called remotely, along with their parameters and return types. On the server side, the server implements this interface and runs a gRPC server to handle calls from clients. On the client side, the client has a stub that provides the same methods as the server.

|![gRPC - Communication between clients and server](/assets/images/posts/grpc-overview.svg)|
|:-:|
|<sup>*Figure 1: gRPC - Communication between clients and server. (Image courtesy: grpc.io)*</sup>|<br/><br/>

gRPC clients and servers can run and talk to each other in any platform (Linux, macos, etc.) and can be written in any of gRPC’s supported languages. For example, we can easily create a gRPC server in Java with clients in Go, Python, or Ruby. Also, the latest Google APIs will have gRPC versions of their interfaces, which will make it easy to add Google features to our applications.

# Protocol buffers

Let's talk about what protocol buffers (also called protobuf) are and how to use them. Protocol buffers is an open-source protocol developed by Google to *serialize* structured data in a way that works in different languages (language-agnostic) and on different platforms.

When working with protocol buffers, the first step is to define the structure of the data that we want to serialize in a *proto file*. Proto file is an ordinary text file with a `.proto` extension. Protocol buffer data is organized into *messages*, where each message is a small logical record of information with a set of name-value pairs called *fields*.

Here’s a simple proto definition example:

```grpc
message Person {
  string name = 1;
  int32 id = 2;
  bool is_married = 3;
}
```

Once we’ve specified our data structures (shown above), we use the protocol buffer compiler `protoc` to generate data access classes in our preferred language from our proto definition. These provide simple accessors (`name()`) and mutators (`set_name()`) for each field, as well as methods to serialize/parse the whole structure to/from raw bytes. For instance, if our chosen language is Java or Python, running the compiler on the example above will generate a class called `Person`. We can then use this class in our application to *populate*, *serialize*, and *retrieve* `Person` protocol buffer messages.

# gRPC usecases

Everyone agrees that gRPC is the best way for internal microservices to talk to each other because it is fast and can be used in different gRPC supported languages. Microservices must agree on the API, data format, error patterns, load balancing, and other things in order to exchange data. Since gRPC lets us describe a service contract in a binary format, programmers can have a standard way to describe these contracts that works with any language and ensures interoperability.


# Benefits of gRPC

TODO

# Weakness of gRPC

TODO

# Frequently asked questions (FAQ)

## What is "g" in gRPC?

We know that Google developed gRPC, but the "g" at the beginning of gRPC doesn't stand for Google. At first, the "g" in "gRPC" was a *recursive acronym* (an acronym that refers to itself) that stood for "gRPC." Later, each release meant something different. In version 1.49, it stands for "gamma," and in version 1.50, it stands for "galley."

## What is protocol buffers?

Protocol buffers (aka protobuf) is an open-source way to *serialize* structured data that works across languages and platforms and can be expanded. Google developed protocol buffers, which are widely used at Google for storing and exchanging all kinds of structured information. It is the foundation for a custom remote procedure call (RPC) system that Google uses for almost all inter-machine (machine-to-machine) communication. Protocol Buffers are similar to the **Apache Thrift**, **Ion** (created by Amazon), or **Microsoft Bond protocols**.

## What is "stub" in distributed computing?

In distributed computing, a **stub** is a piece of code that is used to convert parameters passed between client and server during a remote procedure call (RPC). An RPC allows a client or local computer to remotely call procedures on a server or different computer.

Since the client and server use different [address spaces]({{ site.baseurl }}/rpc/2022/introduction-to-grpc#what-does-address-space-mean), parameters used in a function (procedure) call have to be converted. Otherwise, the values of those parameters couldn't be used because pointers to parameters in one computer's memory would point to different data on the other computer. The client and server may also use different data representations, even for simple parameters (e.g., big-endian versus little-endian for integers). Stubs do this conversion so that the remote server computer perceives the RPC as a local function call.

Stub libraries must be installed on both the client and server side. Client stubs (sometimes called proxy) convert (marshalling) parameters used in function calls and reconvert the result obtained from the server after execution of the function. Server stubs, on the other hand, reconvert parameters passed by clients and convert results back after function execution. Stubs are generated either manually or automatically. Automatic stub generation (more commonly used) uses **Interface Definition Language** (**IDL**) to define client and server interfaces.

## What does address space mean?

An address space is a range of valid memory addresses that a program or process can use. That is, a program or process can access the memory. The memory can be physical or virtual, and it is used to store data and run instructions.

A memory management technique called "virtual memory" can be used to make the size of an address space bigger than the size of physical memory. A virtual memory, which is also called a **page file**, is a physical file on the hard drive that acts like extra RAM or a RAM module. Thus, an address space consists of both physical memory and virtual memory.

## What is endianness?

Texts in different languages are read in different orders. For example, English is read from left to right, but Arabic is read from right to left. In the same way, in computing, endianness is a way to specify the order in which bytes of a word are stored in memory. If one computer reads bytes from left to right and another from right to left, it will be hard for them to talk to each other.

Endianness is represented two ways:

- Big-endian (BE)
- Little-endian (LE)

TODO

## What is  interface definition language (IDL)?

An interface description or definition language (IDL) is a language that is used to define the interface between a client and server process in a distributed system. It makes it possible for two programs written in different languages to talk to each other. IDLs describe an interface in a way that doesn't depend on the language being used (language-independent way). This lets software components that don't use the same language talk to each other.

## How HTTP/2 differs from HTTP/1.1 in terms of performance?

At a high level, HTTP/2:

- It's **binary**, instead of textual
    - Binary protocols are more efficient to parse
    - It's more compact on the wire
- It's fully **multiplexed**, instead of ordered and blocking
    - Unlike HTTP/1.1, HTTP/2 allows multiple request and response messages to be in flight at the same time
- Uses **one connection** for parallelism
    - With HTTP/1, browsers open between four and eight connections per origin and each connection will start a flood of data in the response
    - HTTP/2 allows a client to use just one connection per origin to load a page
- Uses **header compression** to reduce overhead
- Allows **servers to push** responses proactively into client caches
    - Server push potentially allows the server to avoid this round trip of delay by “pushing” the responses it thinks the client will need into its cache
