# DNS Resolution Analysis

## Overview

This project demonstrates how DNS resolution works by capturing and analyzing a DNS query and response using `nslookup` and Wireshark.

For this lab, I used `facebook.com` to observe how a domain name is resolved to an IP address.

## Objectives

- Capture a DNS query and its corresponding response.
- Perform a DNS lookup using `nslookup`.
- Capture and analyze DNS traffic using Wireshark.
- Identify important information in the DNS query and response.
- Understand how a domain name is resolved to an IP address.

## Tools Used

- Windows Command Prompt
- `nslookup`
- Wireshark

---

## 1. DNS Lookup Using `nslookup`

I performed a DNS lookup using the following command:

`nslookup facebook.com`

The lookup returned both IPv4 and IPv6 addresses for `facebook.com`.

**IPv4 address:**

`57.144.38.1`

**IPv6 address:**

`2a03:2880:f310:1:face:b00c:0:25de`

For this analysis, I focused on the IPv4 address because the A record is used to map a domain name to an IPv4 address.

### Evidence

![nslookup result](https://raw.githubusercontent.com/V-Adebayo/dns-lab/main/projects/01-nslookup-facebook.png)

---

## 2. DNS Traffic Capture

I used Wireshark to capture the DNS traffic generated during the lookup.

I applied the following display filter:

`dns`

The capture showed both the DNS query and the corresponding DNS response.

**DNS Query:**

`Standard query 0x0002 A facebook.com`

**DNS Response:**

`Standard query response 0x0002 A facebook.com A 57.144.38.1`

Both packets had the same transaction ID, `0x0002`. This transaction ID helps match the DNS response to the original query.

### Evidence

![DNS query and response](https://raw.githubusercontent.com/V-Adebayo/dns-lab/main/projects/02-wireshark-dns-packets.png)

---

## 3. DNS Query Analysis

I selected the DNS query packet in Wireshark and examined its details.

| Field | Value |
|---|---|
| Domain | `facebook.com` |
| Record Type | A |
| Class | IN |
| Transaction ID | `0x0002` |
| Source Port | `50112` |
| Destination Port | `53` |

The A record query requests the IPv4 address associated with a domain name.

In this case, the computer was asking the DNS resolver for the IPv4 address of `facebook.com`.

### Evidence

![DNS query details](https://raw.githubusercontent.com/V-Adebayo/dns-lab/main/projects/03-dns-query.png)

---

## 4. DNS Response Analysis

I then examined the corresponding DNS response packet.

The response contained an A record that mapped:

`facebook.com → 57.144.38.1`

The response also contained a Time to Live (TTL) of **60 seconds**.

The TTL tells DNS systems how long the record can remain cached before it needs to be queried again.

### Evidence

![DNS response details](https://raw.githubusercontent.com/V-Adebayo/dns-lab/main/projects/04-dns-response.png)

---

## 5. DNS Resolution Process

The DNS resolution process observed during this lab can be summarized as:

    User
      |
      | Requests facebook.com
      v
    Local Computer
      |
      | DNS Query
      | facebook.com
      | Type: A
      v
    DNS Resolver
      |
      | Resolves the domain
      v
    DNS Response
      |
      | A Record
      | 57.144.38.1
      v
    Local Computer
      |
      | Receives IP address
      v
    Destination

### Step 1: Domain Request

The process started when I requested `facebook.com` using `nslookup`.

Because computers use IP addresses to communicate, the domain name needs to be resolved to an IP address.

### Step 2: DNS Query

The computer generated a DNS query requesting the A record for `facebook.com`.

Wireshark showed:

`facebook.com: type A, class IN`

The query used the transaction ID `0x0002`.

### Step 3: DNS Resolver

The DNS query was sent to the configured DNS resolver.

In my capture, the resolver address was:

`fe80::1`

The DNS query used UDP with destination port:

`53`

### Step 4: DNS Resolution

The DNS resolver determines the correct answer for the requested domain.

If the answer is not already in the resolver's cache, the resolver can perform additional lookups through the DNS hierarchy, including:

- Root DNS servers
- TLD DNS servers
- Authoritative DNS servers

These resolver-side lookups were not directly visible in my client-side Wireshark capture.

### Step 5: DNS Response

The DNS resolver returned a response containing an A record:

`facebook.com → 57.144.38.1`

The returned IP address was visible in the Answers section of the DNS response in Wireshark.

The response also contained a TTL of **60 seconds**.

### Step 6: IP Address Obtained

After receiving the DNS response, the computer had the IP address associated with `facebook.com`.

The computer can then use this IP address to establish communication with the destination.

---

## 6. Findings

From this lab, I observed the following:

- `nslookup` successfully resolved `facebook.com`.
- Wireshark captured both the DNS query and response.
- The query requested an A record.
- The DNS response returned the IPv4 address `57.144.38.1`.
- The query and response shared the transaction ID `0x0002`.
- The DNS query used destination port `53`.
- The DNS response contained a TTL of `60 seconds`.
- Wireshark allowed the DNS communication to be examined at the packet level.

---

## 7. Conclusion

This lab gave me practical experience with DNS resolution and packet analysis.

I used `nslookup` to perform a DNS lookup for `facebook.com` and then used Wireshark to capture and analyze the DNS traffic.

From the captured packets, I was able to identify the DNS query, record type, transaction ID, destination port, returned IPv4 address, and TTL.

The lab helped me understand how DNS converts a human-readable domain name into an IP address and how Wireshark can be used to examine DNS communication at the packet level.
