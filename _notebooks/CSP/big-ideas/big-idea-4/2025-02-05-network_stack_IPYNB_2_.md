---
layout: post
toc: True
categories: ['CSP Big Idea 4']
title: Network Stack | Frontend and Backend | HTTP and TCP/IP
description: Frontend and Backend application applied to networking layers
courses: {'csp': {'week': 21}}
type: collab
---

## Network Stack 
The Networking Stack in a GitHub Pages → AWS EC2 Interaction

When making an HTTP request using JavaScript's fetch method, several layers of the networking stack come into play. This blog applies to CSA (Computer Science "A") and CSP (Computer Science Principles) projects. The content applies to our application(s) setup, with GitHub Pages interacting with a backend Python or Java web application on AWS EC2.

***Tools in Use***
- ***Docker:*** Encapsulates the backend application for consistent deployment across environments.
- ***Nginx:*** Manages TCP/IP traffic, load balancing, and routing to backend containers.
- ***Certbot:*** Secures communication by managing SSL/TLS certificates for HTTPS.
- ***SQL Database:*** Stores and retrieves data for CRUD operations.
- ***JavaScript Fetch and Promise Handling:*** Enables asynchronous HTTP requests from the GitHub Pages frontend to the backend.
- ***Python Flask API (RESTful):*** Provides the backend framework for handling HTTP requests, defining routes, and interacting with the database.
- ***Java Spring API (RESTful):*** Similarly provides backend framework for HTTP requests.

### HTTP/DNS (Application Layer)
The Application Layer is responsible for providing network services directly to end-user applications. It facilitates communication between software applications and the network.

#### Frontend (GitHub Pages)
The frontend uses fetch to make HTTP(S) requests to the backend. The domain name of the backend hosted on AWS EC2 is resolved to an IP address using DNS (Domain Name System). The HTTP request is formatted, specifying the method (e.g., GET, POST, PUT, DELETE), headers, and optional body (e.g., JSON payloads for CRUD operations).

#### Backend (AWS EC2 with Docker)
The backend application, running inside Docker containers, processes the request. For database operations:
- **Create**: Inserts new records into the SQL database.
- **Read**: Queries data.
- **Update**: Modifies existing records.
- **Delete**: Removes records.

The backend constructs an HTTP response with a status code, headers, and an optional response body (e.g., JSON data).

**Security**: Certbot ensures that HTTPS (secure HTTP) is enabled by managing SSL/TLS certificates, encrypting all communication.

![Mermaid Diagram](../../../../assets/mermaid/1134df9e7e12b3c7ed14ae86d025ea52d09e4f503d0359f86df301f4c4daac24.png)

### TCP/UDP (Transport Layer)
The Transport Layer is responsible for providing reliable data transfer services to the upper layers. It ensures that data is delivered accurately and in the correct sequence.

#### Request
HTTP(S) requests are transmitted using TCP (Transmission Control Protocol). Nginx handles incoming TCP traffic, routing it to the appropriate Docker container or application based on the request path. A three-way TCP handshake establishes a reliable connection between the client (browser) and the server.

#### Response
The HTTP response is sent back over the same TCP connection. TCP ensures the response is delivered accurately and in order.

### IP (Network Layer)
The Network Layer is responsible for routing packets across network boundaries. It handles the logical addressing and routing of packets to ensure they reach their destination.

#### Request
TCP segments carrying the HTTP request are encapsulated into IP packets with source and destination IP addresses. The packets are routed through the internet via routers to the AWS EC2 server.

#### Response
The server’s IP address sends IP packets back to the client, carrying the HTTP response data.

**AWS Infrastructure**: AWS handles routing and load balancing as needed within its data center.

### Physical Layer
The Physical Layer is responsible for the transmission and reception of raw bit streams over a physical medium. It converts data into electrical, optical, or radio signals, which are representations of binary data (0s and 1s).


#### Request & Response
IP packets are converted into physical signals appropriate for the medium (e.g., Ethernet, Wi-Fi, or fiber optics). These signals, representing binary data, traverse physical infrastructure, including cables, wireless access points, and routers.

This addition helps to clarify that the physical signals are essentially binary data being transmitted over various media.
