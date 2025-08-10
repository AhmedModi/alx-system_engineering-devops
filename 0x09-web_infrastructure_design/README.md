## 📄 Web Infrastructure Design

### 0. Simple Web Stack

This diagram illustrates a **basic web infrastructure** for hosting a website that runs on a single server.

**Components:**

* **1 Server** running Ubuntu.
* **Nginx** as the web server.
* **Application code** (e.g., PHP or Python).
* **MySQL database** to store dynamic data.
* **Domain Name** configured via DNS (A record for `www.foobar.com` → Server IP `8.8.8.8`).

**Workflow:**

1. User enters the domain in their browser.
2. DNS resolves the domain to the server’s IP address.
3. Nginx serves static files and forwards dynamic requests to the application server.
4. Application interacts with MySQL for data.
5. Response is sent back to the user.

**Issues:**

* **SPOF:** If the single server fails, the site is down.
* **Maintenance Downtime:** Server restarts cause downtime.
* **No Scalability:** Can’t handle high traffic efficiently.

---

### 1. Distributed Web Infrastructure

This setup adds redundancy, load balancing, and a database replication setup.

**Components:**

* **2 Web Servers** with Nginx and application code.
* **1 Load Balancer** (HAProxy) distributing traffic (Round Robin).
* **1 Primary MySQL database** for writes.
* **1 Replica MySQL database** for read queries.

**Workflow:**

1. DNS resolves `www.foobar.com` to the Load Balancer’s IP.
2. Load Balancer routes requests to Server 1 or Server 2.
3. Servers handle requests and query/write to the appropriate database.
4. Primary DB replicates to Replica DB.

**Issues:**

* **SPOF:** If Load Balancer fails, all access is lost.
* **No HTTPS:** All traffic is in plain text.
* **No Monitoring:** Failures could go unnoticed.

---

### 2. Secured and Monitored Web Infrastructure

This improves on Task 1 by adding **security, encryption, and monitoring**.

**Components Added:**

* **3 Firewalls:**

  * Between the internet and Load Balancer.
  * Between Load Balancer and web servers.
  * Between web servers and database servers.
  * *Purpose:* Restrict unauthorized access and filter malicious traffic.

* **SSL Certificate (HTTPS):**

  * Encrypts traffic between users and the Load Balancer.
  * Protects sensitive data from eavesdropping.

* **Monitoring Clients:**

  * Installed on each server.
  * Send logs and metrics to a service like Sumo Logic.
  * *Purpose:* Track uptime, CPU, memory usage, QPS, and detect anomalies.

**Workflow:**

1. User connects via HTTPS → Load Balancer terminates SSL.
2. Requests are routed securely to web servers via internal network.
3. Web servers handle requests and interact with DB.
4. Monitoring agents collect logs, metrics, and alerts in real-time.

**Issues:**

* **SSL Termination at Load Balancer:** Traffic between Load Balancer and servers is unencrypted.
* **Single Primary DB:** Still a SPOF for write operations.
* **Identical Server Components:** Makes it harder to scale independently (tight coupling).
