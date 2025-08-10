### 0. Simple Web Stack

This diagram illustrates a **basic web infrastructure** for hosting a website that runs on a single server using **LAMP stack** components.

**Components:**

* **1 Server** running Ubuntu.
* **Nginx** as the web server.
* **Application code** built with PHP.
* **MySQL database** to store dynamic data.
* **Domain Name** configured via DNS to point to the server’s public IP address.

**Workflow:**

1. User enters the domain name in their browser.
2. DNS resolves the domain to the server’s public IP.
3. Nginx serves static files directly and passes PHP requests to the application code.
4. The application interacts with the MySQL database to fetch/store data.
5. Response is sent back to the browser for rendering.

**Purpose:**
This setup is suitable for small-scale projects or low-traffic websites but is a **single point of failure** — if the server goes down, the website becomes unavailable.

---

### 1. Distributed Web Infrastructure

This diagram shows a **scalable and fault-tolerant infrastructure** using multiple servers and load balancing.

**Components:**

* **2 Web Servers** running Nginx to handle client requests.
* **1 Load Balancer** (HAProxy) distributing traffic between the web servers.
* **1 Application Database Server** (MySQL).
* **Firewall** for securing the network.
* **Monitoring** tools like Datadog for tracking system health and performance.

**Workflow:**

1. User enters the website’s domain in their browser.
2. DNS resolves the domain to the **Load Balancer’s IP**.
3. The Load Balancer routes the request to one of the available web servers.
4. The web server processes the request, possibly querying the MySQL database.
5. The server returns the processed result to the user via the Load Balancer.

**Advantages:**

* **Scalability:** Requests are distributed across multiple servers.
* **Fault Tolerance:** If one server fails, the other can handle traffic.
* **Security:** Firewalls filter unauthorized access.
* **Monitoring:** Ensures issues are detected early and resolved quickly.

---
