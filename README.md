# DNS + Web Server Hosting Project 🌐

## 📌 Overview
This project demonstrates the configuration of a **DNS server (BIND)** and a **Web server (Apache)** on Linux (RHEL/Rocky Linux/CentOS).  
The DNS server resolves a custom domain name (`www.myproject.local`) to the web server’s IP, allowing clients to access the hosted application via domain name instead of IP.

This project is aligned with **RHCSA exam objectives** and highlights real-world **Linux system administration and DevOps networking skills**.

---

## 🏗 Architecture

[ Client ] --- DNS Query ---> [ DNS Server (BIND) ] --- Forward ---> [ Web Server (Apache) ]

yaml
Copy code

- **DNS Server** → Provides name resolution for `myproject.local` domain  
- **Web Server** → Hosts a static web page  
- **Clients** → Resolve domain via DNS server and access the site  

---

## ⚙️ Setup Instructions

### 1. Prerequisites
- 2 Linux VMs (or servers):
  - **VM1: DNS Server**
  - **VM2: Web Server**
- Packages: `bind`, `bind-utils`, `httpd`, `curl`, `dig`

---

### 2. Configure Web Server (Apache)

On **VM2** (Web Server):

```bash
sudo yum install httpd -y
echo "<h1>Welcome to My Web Project</h1>" | sudo tee /var/www/html/index.html
sudo systemctl enable --now httpd
Verify locally:

bash
Copy code
curl http://localhost
3. Configure DNS Server (BIND)
On VM1 (DNS Server):

bash
Copy code
sudo yum install bind bind-utils -y
Edit /etc/named.conf
conf
Copy code
options {
    listen-on port 53 { any; };
    directory     "/var/named";
    allow-query   { any; };
    recursion yes;
};

zone "myproject.local" IN {
    type master;
    file "myproject.local.zone";
    allow-update { none; };
};
Create zone file /var/named/myproject.local.zone
dns
Copy code
$TTL 86400
@   IN SOA  ns1.myproject.local. root.myproject.local. (
        2025083001 ; Serial
        3600       ; Refresh
        1800       ; Retry
        1209600    ; Expire
        86400 )    ; Minimum TTL

    IN  NS  ns1.myproject.local.
ns1 IN  A   192.168.64.130    ; DNS Server
www IN  A   192.168.64.150    ; Web Server
Restart DNS
bash
Copy code
sudo systemctl enable --now named
sudo systemctl restart named
4. Configure Client
On client machine (can be DNS server itself, web server, or another VM):

Edit /etc/resolv.conf:

conf
Copy code
nameserver 192.168.64.130   # DNS Server IP
5. Testing
Check DNS resolution:

bash
Copy code
dig www.myproject.local
Expected:

less
Copy code
;; ANSWER SECTION:
www.myproject.local.   86400   IN  A   192.168.64.150
Access website:

bash
Copy code
curl http://www.myproject.local
Expected:

css
Copy code
<h1>Welcome to My Web Project</h1>
✅ Expected Output
dig www.myproject.local → Returns 192.168.64.150

curl http://www.myproject.local → Displays web page content

Opening http://www.myproject.local in browser shows the HTML page

📂 Project Structure
pgsql
Copy code
dns-webserver-project/
├── README.md
├── dns-server/
│   ├── named.conf
│   ├── myproject.local.zone
│   └── resolv.conf.sample
├── web-server/
│   ├── index.html
│   └── httpd.conf.sample
└── screenshots/
    ├── dig_output.png
    ├── curl_output.png
    └── browser_view.png
🛠️ Technologies Used
Linux (RHEL/Rocky/CentOS)

BIND (DNS Server)

Apache HTTPD (Web Server)

Networking Tools: dig, nslookup, curl, firewalld

🎯 Learning Outcomes
Configured a private DNS server with forward lookup zones

Deployed and tested a web server accessible via DNS

Applied RHCSA-level system administration skills

Gained practical experience in Linux networking & DevOps fundamentals

📌 Author
👤 Your Name
📧 your.email@example.com
💼 LinkedIn Profile

yaml
Copy code

---

Do you also want me to generate a **one-line GitHub description + tags** (so when s# dns-webhosting
