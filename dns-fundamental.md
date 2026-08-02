# DNS Fundamentals

## Overview

The **Domain Name System (DNS)** is a distributed naming system that translates human-readable domain names into IP addresses. It acts as the Internet's phonebook, allowing users to access websites using names instead of numerical IP addresses.

Example:

```
www.google.com
        ↓
142.250.190.78
```

---

# Learning Objectives

After studying DNS Fundamentals, I can:

- Explain the purpose of DNS.
- Describe how DNS resolution works.
- Differentiate between recursive and authoritative DNS servers.
- Identify common DNS record types and their functions.
- Explain DNS caching and Time To Live (TTL).
- Understand the role of Root, TLD, and Authoritative DNS servers.
- Describe basic DNS security threats.

---

# Why DNS is Important

Without DNS, users would need to remember IP addresses for every website they visit. DNS simplifies communication by converting domain names into IP addresses automatically.

---

# DNS Resolution Process

When a user enters a website into a browser, DNS follows these steps:

1. The browser checks its local DNS cache.
2. The operating system checks its DNS cache.
3. The request is sent to a recursive DNS resolver.
4. The recursive resolver queries a Root DNS Server.
5. The Root Server refers the resolver to the appropriate Top-Level Domain (TLD) server.
6. The TLD Server refers the resolver to the Authoritative DNS Server.
7. The Authoritative DNS Server returns the requested IP address.
8. The recursive resolver sends the IP address back to the client.
9. The browser connects to the web server.

---

# Types of DNS Servers

## Recursive DNS Resolver

The recursive resolver receives DNS requests from clients and performs all necessary lookups until it finds the requested IP address. It also caches responses to improve performance.

**Responsibilities**

- Receives client queries
- Performs DNS lookups
- Caches DNS responses
- Returns the final answer to the client

---

## Root DNS Server

The Root DNS Server is the first server contacted during DNS resolution.

**Responsibilities**

- Directs queries to the correct Top-Level Domain (TLD) server
- Does not store website IP addresses

---

## Top-Level Domain (TLD) Server

A TLD server manages domains based on their extensions.

Examples include:

- .com
- .org
- .net
- .edu
- .gov

Its role is to direct queries to the correct authoritative DNS server.

---

## Authoritative DNS Server

The authoritative DNS server stores the official DNS records for a domain and provides the final answer to DNS queries.

---

# Common DNS Record Types

| Record | Purpose |
|---------|---------|
| **A** | Maps a hostname to an IPv4 address |
| **AAAA** | Maps a hostname to an IPv6 address |
| **CNAME** | Creates an alias for another hostname |
| **MX** | Specifies the mail server for a domain |
| **NS** | Identifies the authoritative name servers |
| **PTR** | Performs reverse DNS lookups (IP address → Hostname) |
| **TXT** | Stores text information such as SPF, DKIM, and domain verification records |
| **SOA** | Contains administrative information about the DNS zone |

---

# DNS Caching

DNS caching temporarily stores previously resolved DNS records to reduce lookup times and improve performance.

### Benefits

- Faster website access
- Reduced DNS traffic
- Lower workload on DNS servers

---

# Time To Live (TTL)

**Time To Live (TTL)** defines how long a DNS record remains in cache before it expires and must be queried again.

A shorter TTL allows DNS changes to propagate more quickly, while a longer TTL reduces the number of DNS queries.

---

# DNS Protocols and Ports

DNS primarily uses:

| Protocol | Port | Purpose |
|----------|------|---------|
| UDP | 53 | Standard DNS queries |
| TCP | 53 | Zone transfers and large DNS responses |

UDP is preferred because it is faster and has lower overhead than TCP.

---

# Forward and Reverse DNS Lookup

### Forward Lookup

Converts a domain name into an IP address.

```
www.example.com
        ↓
192.168.1.10
```

---

### Reverse Lookup

Converts an IP address back into a hostname using a PTR record.

```
192.168.1.10
        ↓
www.example.com
```

---

# DNS Security Concepts

## DNS Cache Poisoning

An attacker inserts malicious DNS records into a resolver's cache, redirecting users to fraudulent or malicious websites.

---

## DNS Tunneling

Attackers use DNS queries and responses to secretly transmit data or communicate with command-and-control (C2) servers.

---

## Domain Generation Algorithm (DGA)

Some malware generates thousands of random domain names until one successfully connects to an attacker-controlled server.

Example:

```
x83jk2.com
aj72lp.net
qwe91fd.org
```

---

# Common Indicators of Suspicious DNS Activity

- Excessive DNS requests
- Random or algorithmically generated domain names
- High number of failed DNS lookups (NXDOMAIN responses)
- Frequent TXT record queries
- Unusually long subdomains
- Communication with newly registered domains
- High DNS traffic outside normal business hours

---

# Key Takeaways

- DNS translates domain names into IP addresses.
- Recursive resolvers perform DNS lookups for clients.
- Authoritative servers provide the official DNS records.
- Root and TLD servers help locate the authoritative server.
- Common DNS records include A, AAAA, CNAME, MX, NS, PTR, TXT, and SOA.
- DNS caching improves performance.
- TTL determines how long DNS records remain cached.
- DNS primarily uses UDP port 53 and TCP port 53 when necessary.
- Monitoring DNS activity is important for detecting malicious behavior.

---


