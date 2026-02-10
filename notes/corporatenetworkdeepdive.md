# Corporate Network Deep Dive

## “How Applications Run Inside Real Companies”

# 🎯 Session Objective

By the end of this session, students will understand:

* What a corporate network looks like
* LAN vs WAN inside companies
* VLAN
* Proxy servers
* VPN
* DMZ
* Internal vs External servers
* Active Directory basics
* How production systems are structured


# 🧠 Opening

Last session, we learned:

* IP
* DNS
* Firewall
* Ports

Today, we answer a bigger question:

👉 What does networking look like inside Infosys, TCS, Google, or a bank?

Do they just connect laptops to WiFi and deploy NodeJS apps?

No.

Corporate networks are layered, secure, and segmented.

Let’s break it down.

# 🏢 1️⃣ What is a Corporate Network?

A corporate network is:

> A controlled, secure internal network used by an organization.

Basic View:

```
              INTERNET
                  |
              [Firewall]
                  |
              [Core Router]
                  |
      --------------------------------
      |              |              |
   HR Dept        Dev Dept       Finance Dept
      |              |              |
   LAN A           LAN B          LAN C
```

This is NOT a home network.

It has:

* Segmentation
* Access control
* Monitoring
* Policies


# 🌐 2️⃣ LAN vs WAN (Inside Corporate)

### LAN – Local Area Network

Within one building.

Example:

```
[Dev Laptop 1] \
[Dev Laptop 2] ---> [Switch] ---> [Internal Server]
[Dev Laptop 3] /
```

### WAN – Wide Area Network

Connects multiple office locations.

Example:

```
[Chennai Office] \
                    ---> [Corporate WAN Backbone] ---> [Mumbai Office]
[Delhi Office]   /
```

Large companies connect branches through leased lines or MPLS.

# 🧩 3️⃣ VLAN (Virtual LAN)

Important concept.

Question:
Why should HR computers talk directly to Finance servers?

They should NOT.

So we use VLAN.

VLAN logically separates networks even if hardware is same.

Example:

```
Same Physical Switch
---------------------------------
| Port 1 → VLAN 10 (HR)        |
| Port 2 → VLAN 10 (HR)        |
| Port 3 → VLAN 20 (Finance)   |
| Port 4 → VLAN 30 (Dev)       |
---------------------------------
```

Logically separated:

```
VLAN 10 (HR)
VLAN 20 (Finance)
VLAN 30 (Development)
```

Even if connected to same switch,
They behave like different networks.

Security + control.

# 🔥 4️⃣ Multi-Layer Firewall Architecture

Corporate network does NOT have one firewall.

It has multiple layers.

```
                  INTERNET
                      |
                [Edge Firewall]
                      |
                [DMZ Zone]
                      |
                [Internal Firewall]
                      |
                [Internal Network]
```

# 🛡 5️⃣ What is DMZ?

DMZ = Demilitarized Zone

Public-facing servers are placed here.

Example:

```
             INTERNET
                 |
          ----------------
          |              |
       [Web Server]   [Mail Server]
          |
         DMZ
          |
     Internal Firewall
          |
     Database Server
```

Why?

Because:

If hacker attacks web server,
He should NOT directly access database.

DMZ isolates public servers from internal network.

# 🧑‍💻 6️⃣ Internal vs External Servers

Inside company:

### External Servers (DMZ)

* Public website
* Public API
* Email gateway

### Internal Servers

* HR software
* Internal dashboards
* Dev Git server
* Database servers

Diagram:

```
INTERNET
   |
[Firewall]
   |
[DMZ] ---- Web Server
   |
[Internal Network]
   |
[Database Server]
[Git Server]
[Employee Portal]
```

Internal servers are not directly internet accessible.

# 🔐 7️⃣ Proxy Server (Very Important)

In corporate environment:

You cannot directly browse internet.

Traffic goes through proxy.

```
[Employee Laptop]
        |
     [Proxy Server]
        |
     INTERNET
```

Proxy does:

* Logs websites
* Blocks restricted sites
* Scans for malware
* Applies policies

If proxy down:
No browsing.

# 🌍 8️⃣ VPN (Work From Home)

When employees work from home:

They connect using VPN.

```
[Home Laptop]
      |
  Encrypted Tunnel
      |
  [Corporate Firewall]
      |
  [Internal Network]
```

VPN creates:

Secure encrypted tunnel over internet.

So laptop behaves as if inside office LAN.

# 🏢 9️⃣ Active Directory (Basic Idea)

Corporate networks use centralized identity system.

Example:
When you log into office laptop:

```
Username: john.doe
Password: *****
```

Authentication happens against:

👉 Active Directory (AD)

AD controls:

* Login access
* File permissions
* Group policies
* Software access

Diagram:

```
[Employee Laptop]
       |
[Domain Controller / AD Server]
       |
   Access Granted
```

This is why:
One password works everywhere in company.

# 🧠 1️⃣0️⃣ Corporate Application Deployment Architecture

Let’s connect this to software development.

Typical production setup:

```
                 INTERNET
                     |
               [Load Balancer]
                     |
            ---------------------
            |         |         |
        [App1]     [App2]    [App3]
            |
         [Internal Firewall]
            |
        [Database Cluster]
```

Key components:

* Load balancer
* Multiple app servers
* Internal DB
* Restricted access

Developers DO NOT directly connect to production DB.

Security rules apply.

# 🚨 1️⃣1️⃣ Network Monitoring

Corporate networks use:

* SIEM tools
* IDS/IPS
* Traffic monitoring
* Logging systems

If unusual traffic detected:

Alert triggered.

Example:

```
1000 failed login attempts
→ Security alert
→ Account locked
```

# 🧩 Real-World Debug Thinking

If production API not working:

Think layer by layer:

```
DNS correct?
Load balancer healthy?
Firewall blocking?
App server running?
Database reachable?
Network ACL blocking?
```

Corporate debugging is multi-layered.

# 🎓 Closing – Transflower Mentor Tone

Students,

In college, you deploy:

```
localhost:3000
```

In company, your app lives inside:

* VLAN
* Firewall
* DMZ
* Load balancer
* Proxy
* VPN
* Active Directory
* Monitoring systems

Now you understand:

Why “It works on my machine” means nothing.

Because corporate infrastructure is a different world.

From today:
You don’t just build apps.

You design systems that survive inside corporate networks.

That is engineering maturity.

# Deep Dive into HTTPS, SSL & TLS

## “What Really Happens Behind the 🔒 Lock Icon?”

# 🎯 Session Objective

By the end of this session, students will understand:

* Why HTTP is insecure
* What HTTPS solves
* SSL vs TLS difference
* Symmetric vs Asymmetric encryption
* Public key / Private key
* TLS Handshake step-by-step
* Certificates & Certificate Authorities
* Real production debugging scenarios

# 🧠 Opening Question

You type:

```
https://google.com
```

You see a lock icon 🔒

But what is actually happening?

Is data just magically secure?

No.

Today we break it down like engineers.

# 🌐 1️⃣ Why HTTP is Insecure

Normal HTTP:

```
Client  -------------------->  Server
        (Plain Text Data)
```

If you login:

```
username=rahul
password=123456
```

Anyone in between can read it.

This is called:

👉 Man-in-the-Middle Attack (MITM)

Example:

```
Client → Hacker → Server
```

Hacker sees everything.

HTTP = No encryption.

# 🔐 2️⃣ What HTTPS Does

HTTPS = HTTP + Encryption

Now communication looks like:

```
Client  ====================>  Server
         (Encrypted Data)
```

Even if hacker intercepts:

```
ajd83hsk29skk39sls82ks
```

He cannot understand.

# 📚 3️⃣ SSL vs TLS

History:

* SSL (Secure Sockets Layer) → Old
* TLS (Transport Layer Security) → New & Secure

Today:
We technically use TLS.

But people still say SSL certificate.

So:

SSL = Old term
TLS = Actual modern protocol

# 🔑 4️⃣ Two Types of Encryption

To understand TLS, you must understand:

### 1️⃣ Symmetric Encryption

Same key used to encrypt and decrypt.

```
Shared Secret Key
```

Example:

```
Client & Server both know: KEY123
```

Pros:

* Fast

Problem:
How do they securely share that key?

### 2️⃣ Asymmetric Encryption

Uses 2 keys:

* Public Key
* Private Key

Diagram:

```
Public Key  → Shared with everyone
Private Key → Kept secret on server
```

If encrypted with Public Key,
Only Private Key can decrypt.


# 🧠 5️⃣ How TLS Solves the Problem

TLS uses:

* Asymmetric encryption (for handshake)
* Symmetric encryption (for actual data)

Because:

* Asymmetric = secure but slow
* Symmetric = fast

Best of both worlds.


# 🤝 6️⃣ TLS Handshake – Step by Step

This is the most important part.

Let’s simulate:

You open:

```
https://example.com
```

## Step 1: Client Hello

```
Client → Server:
"Hello, I support these encryption methods"
```

Includes:

* TLS version
* Cipher suites
* Random number

## Step 2: Server Hello

```
Server → Client:
"Okay, we will use TLS 1.3"
"Here is my certificate"
```

Certificate contains:

* Server public key
* Domain name
* Issuer (CA)


# 📜 7️⃣ What is a Certificate?

Certificate proves:

> “This public key really belongs to example.com”

Structure:

```
Certificate
------------
Domain: example.com
Public Key: XYZ123
Issued By: DigiCert
Signature: ...
```

# 🏢 8️⃣ Certificate Authority (CA)

CA is trusted third party.

Examples:

* DigiCert
* GlobalSign
* Let's Encrypt

Flow:

```
Server → CA:
"Please verify me"

CA verifies domain ownership

CA signs certificate
```

Browser trusts CA.

So if CA signs certificate,
Browser trusts server.

# 🔎 9️⃣ Certificate Verification

When browser receives certificate:

It checks:

1. Is CA trusted?
2. Is certificate expired?
3. Does domain match?
4. Is signature valid?

If all pass:

🔒 Secure connection established.

If not:

⚠️ “Your connection is not private”

# 🔑 1️⃣0️⃣ Key Exchange

Now comes critical part.

Client generates:

```
Pre-Master Secret
```

Encrypts it using:

Server’s Public Key

```
Encrypted Secret → Server
```

Server decrypts using:

Private Key

Now both have shared secret.

From this:

They generate:

```
Session Key
```

Now communication switches to:

👉 Symmetric encryption (fast)

# 🔁 Final Encrypted Communication

Now data flows:

```
Client  =======================  Server
         Encrypted using
         Session Key
```

Hacker sees only encrypted data.

# 📦 Full TLS Flow Diagram

```
Client                                Server
  |                                      |
  | --- Client Hello ------------------> |
  |                                      |
  | <--- Server Hello + Certificate ---- |
  |                                      |
  | --- Encrypted Pre-Master Secret ---> |
  |                                      |
  | ===== Secure Encrypted Channel ===== |
  |                                      |
```

After handshake:
All HTTP data encrypted.

# 🔐 1️⃣1️⃣ What Happens Without Valid Certificate?

If certificate:

* Expired
* Self-signed
* Domain mismatch

Browser shows:

```
⚠️ Not Secure
```

Because trust chain broken.

# 🏢 1️⃣2️⃣ Corporate Reality

In corporate:

HTTPS used for:

* APIs
* Microservices
* Internal dashboards
* Payment systems

Production setup:

```
Internet
   |
[Load Balancer (TLS termination)]
   |
[App Servers]
   |
[Internal Network]
```

Sometimes TLS ends at load balancer.

Then internal traffic may or may not be encrypted.

# 🚨 Real-World Debug Scenarios

### Scenario 1:

Website shows “Not Secure”

Possible reasons:

* Certificate expired
* Wrong domain certificate
* Missing intermediate certificate

### Scenario 2:

API works on HTTP but fails on HTTPS

Possible:

* Mixed content issue
* TLS version mismatch
* Port 443 blocked

### Scenario 3:

SSL handshake failed

Reasons:

* Client supports TLS 1.2 only
* Server requires TLS 1.3
* Cipher mismatch


# 🔥 HTTPS vs HTTP Comparison

| Feature         | HTTP    | HTTPS     |
| --------------- | ------- | --------- |
| Encryption      | ❌ No   | ✅ Yes   |
| MITM Protection | ❌      | ✅       |
| Data Integrity  | ❌      | ✅       |
| SEO Ranking     | Lower   | Higher    |
| Trust           | No lock | Lock icon |

# 🎓 Closing – Transflower Mentor Tone

Students,

When you see 🔒 in browser:

It means:

* Certificate verified
* Public key validated
* Session key exchanged
* Data encrypted
* Integrity protected

That small lock icon represents:
Mathematics.
Cryptography.
Trust hierarchy.
Network security.
Global infrastructure.

From today:
You don’t just “enable HTTPS”.
You understand how it works.
That’s engineering depth.