## What is a Load Balancer?
A load balancer is a solution that acts as a traffic proxy and distributes network or application traffic across endpoints on several servers. Load balancers are used to distribute capacity during peak traffic times, and to increase the reliability of applications. They improve the overall performance of applications by decreasing the burden on individual services or clouds and distributing the demand across different compute surfaces to help maintain application and network sessions. 

### Benefits of Load Balancing
Load balancing directs and controls internet traffic between the application servers and their visitors or clients. As a result, it improves an application’s availability, scalability, security, and performance.

__1. Application availability__

Server failure or maintenance can increase application downtime, making your application unavailable to visitors. Load balancers increase the fault tolerance of your systems by automatically detecting server problems and redirecting client traffic to available servers. You can use load balancing to make these tasks easier:

- Run application server maintenance or upgrades without application downtime
- Provide automatic disaster recovery to backup sites
- Perform health checks and prevent issues that can cause downtime.

__2. Application scalability__

You can use load balancers to direct network traffic intelligently among multiple servers. Your applications can handle thousands of client requests because load balancing does the following:

- Prevents traffic bottlenecks at any one server
- Predicts application traffic so that you can add or remove different servers if needed
- Adds redundancy to your system so that you can scale with confidence.

__3. Application security__

Load balancers come with built-in security features to add another layer of security to your internet applications. They are a useful tool to deal with distributed denial of service attacks, in which attackers flood an application server with millions of concurrent requests that cause server failure. Load balancers can also do the following:

- Monitor traffic and block malicious content
- Automatically redirect attack traffic to multiple backend servers to minimize the impact
- Route traffic through a group of network firewalls for additional security.

__4. Application performance__

Load balancers improve application performance by increasing response time and reducing network latency. They perform several critical tasks such as the following:

- Distribute the load evenly between servers to improve application performance
- Redirect client requests to a geographically closer server to reduce latency
- Ensure the reliability and performance of physical and virtual computing resources

### What are load-balancing techniques?
A load balancing algorithm is the set of rules that a load balancer follows to determine the best server for each of the different client requests. Load-balancing algorithms fall into two main categories.

__Static load balancing__

Static load balancing algorithms follow fixed rules and are independent of the current server state. The following are examples of static load balancing.

_Round-robin method_

Servers have IP addresses that tell the client where to send requests. The IP address is a long number that is difficult to remember. To make it easy, a Domain Name System maps website names to servers. When you enter aws.amazon.com into your browser, the request first goes to our name server, which returns our IP address to your browser.

In the round-robin method, an authoritative name server does the load balancing instead of specialized hardware or software. The name server returns the IP addresses of different servers in the server farm turn by turn or in a round-robin fashion.

_Weighted round-robin method_

In weighted round-robin load balancing, you can assign different weights to each server based on their priority or capacity. Servers with higher weights will receive more incoming application traffic from the name server.

_IP hash method_

In the IP hash method, the load balancer performs a mathematical computation, called hashing, on the client's IP address. It converts the client IP address to a number, which is then mapped to individual servers.

__Dynamic load balancing__

Dynamic load balancing algorithms examine the current state of the servers before distributing traffic. The following are some examples of dynamic load-balancing algorithms.

_Least connection method_

A connection is an open communication channel between a client and a server. When the client sends the first request to the server, they authenticate and establish an active connection with each other. In the least connection method, the load balancer checks which servers have the fewest active connections and sends traffic to those servers. This method assumes that all connections require equal processing power for all servers.

_Weighted least connection method_

Weighted least connection algorithms assume that some servers can handle more active connections than others. Therefore, you can assign different weights or capacities to each server, and the load balancer sends the new client requests to the server with the least connections by capacity.

_Least response time method_

The response time is the total time that the server takes to process the incoming requests and send a response. The least response time method combines the server response time and the active connections to determine the best server. Load balancers use this algorithm to ensure faster service for all users.

_Resource-based method_

In the resource-based method, load balancers distribute traffic by analyzing the current server load. Specialized software called an agent runs on each server and calculates usage of server resources, such as its computing capacity and memory. Then, the load balancer checks the agent for sufficient free resources before distributing traffic to that server.

## What are the types of load balancing?

We can classify load balancing into three main categories depending on what the load balancer checks in the client request to redirect the traffic.

__Application load balancing__

Complex modern applications have several server farms with multiple servers dedicated to a single application function. Application load balancers look at the requested content, such as HTTP headers or SSL session IDs, to redirect traffic. 

For example, an e-commerce application has a product directory, shopping cart, and checkout functions. The application load balancer sends requests for browsing products to servers that contain images and videos but do not need to maintain open connections. By comparison, it sends shopping cart requests to servers that can maintain many client connections and save cart data for a long time.

__Network load balancing__

Network load balancers examine IP addresses and other network information to redirect traffic optimally. They track the source of the application traffic and can assign a static IP address to several servers. Network load balancers use the static and dynamic load balancing algorithms described earlier to balance server load.

__Global server load balancing__

Global server load balancing occurs across several geographically distributed servers. For example, companies can have servers in multiple data centers, in different countries, and third-party cloud providers around the globe. In this case, local load balancers manage the application load within a region or zone. They attempt to redirect traffic to a server destination that is geographically closer to the client. They might redirect traffic to servers outside the client’s geographic zone only in case of server failure.

__DNS load balancing__

In DNS load balancing, you configure your domain to route network requests across a pool of resources on your domain. A domain can correspond to a website, a mail system, a print server, or another service that is made accessible through the internet. DNS load balancing helps maintain application availability and balance network traffic across a globally distributed pool of resources. 
