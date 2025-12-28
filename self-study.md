# Apache Load Balancer – Self Study Guide

This guide explains **load balancing**, the difference between **Layer 4 (L4)** and **Layer 7 (L7)** load balancers, and includes notes for further self-study on Apache’s `mod_proxy_balancer`.

---

## What is a Load Balancer?

A **load balancer** acts like a traffic manager for your servers. Imagine cars trying to enter a highway: if there’s only one lane, traffic jams happen. If there are multiple lanes and a traffic controller directing cars, the flow is smooth. Similarly, a load balancer distributes incoming network traffic across multiple servers to:

- Avoid overloading any single server  
- Ensure high availability  
- Improve performance  

---

## Key Concepts of a Load Balancer

1. **Traffic Distribution**: Sends requests to multiple servers instead of just one.  
2. **Prevent Overload**: Protects servers from crashing due to too much traffic.  
3. **Failover / Backup**: If a server goes down, traffic is rerouted to healthy servers.  
4. **Health Checks**: Continuously monitors servers and skips any that are unavailable.  
5. **Session Stickiness**: Keeps a user connected to the same server during a session (useful for apps storing temporary session data).

---

## Types of Load Balancers

Load balancers are classified based on the **layer of the OSI model** they operate on:

### 1. L4 – Network Load Balancer (Layer 4)

- Works with **IP addresses** and **port numbers** (basic traffic info)  
- Very **fast** because it does not inspect the content of requests  
- Ideal for **speed-sensitive applications** like video streaming or messaging  

**Example Use Case**: Distributing TCP or UDP traffic evenly across servers.

---

### 2. L7 – Application Load Balancer (Layer 7)

- Looks **inside the request** at the URL, headers, cookies, etc.  
- Can make **intelligent routing decisions** based on request content  
- Slightly slower than L4 due to deeper inspection  
- Ideal for **websites and web apps** where content-aware routing matters  

**Example Use Case**: Sending login page requests to one server and media content to another.

---

### Key Differences Between L4 and L7

| Feature                  | L4 Network Load Balancer                     | L7 Application Load Balancer                |
|--------------------------|----------------------------------------------|---------------------------------------------|
| **Traffic Handling**      | IP address, port numbers                      | Content (URLs, headers, cookies)           |
| **Speed**                 | Very fast                                    | Slightly slower                             |
| **Use Case**              | High-speed, simple traffic distribution      | Content-based routing for websites/apps    |
| **Supported Protocols**   | TCP, UDP                                     | HTTP, HTTPS                                 |

**Analogy**:  
- **L4**: A traffic cop directing cars based only on which road/lane they’re on.  
- **L7**: A smarter traffic cop checking where each car wants to go before choosing the lane.

---

## Side Self-Study

1. **`mod_proxy_balancer` Module**:  
   - The `mod_proxy_balancer` module in Apache allows you to configure Apache as a **load balancer** for backend servers.  
   - **Key directives**:  
     - `BalancerMember`: Defines the backend servers that will receive traffic.  
     - `ProxySet`: Configures load balancing parameters like `lbmethod` (how requests are distributed) or `stickysession`.  
     - `ProxyPass`: Forwards requests from Apache to the backend balancer.  
   - **Why it matters**: Using this module, you can distribute traffic efficiently, perform health checks, and manage failover.

2. **Sticky Sessions (Session Persistence)**:  
   - **Sticky sessions** ensure that a user is always routed to the same backend server for the duration of their session.  
   - **Why it’s needed**: Some web applications store temporary session data (like shopping carts or login info) on a single server. If requests jump between servers, users may lose data or have inconsistent experiences.  
   - **When to use**: Use sticky sessions when backend servers store session-specific data locally. If your application uses a shared session store (like Redis or a database), sticky sessions are usually not needed.

---

## Next Steps

- Set up a small Apache load balancer in a lab environment.  
- Test both **L4** and **L7** traffic distribution and observe the differences.  
- Experiment with **sticky sessions** using `mod_proxy_balancer` and see how it affects user experience when session data is stored on a single server.
