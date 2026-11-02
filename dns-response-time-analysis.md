# DNS Resolution Process and How DNS Servers Work

## 1. Introduction

The Domain Name System (DNS) helps devices find the IP addresses associated with domain names. People usually remember website names such as `google.com`, but computers use IP addresses to communicate across networks.

DNS resolution is the process of finding the IP address associated with a domain name.

## 2. What Is DNS Resolution?

DNS resolution happens when a device needs to find the IP address for a domain name.

For example, when a user enters `google.com` into a browser, the device needs to obtain the appropriate IP address before it can establish a connection to the website.

The device may use a previously cached DNS record. If a valid cached record is unavailable, it may send a request to a DNS resolver.

## 3. The Main Components of DNS

### DNS Client

The DNS client is the device or application requesting DNS information. For example, a Windows computer may need to resolve a website's domain name.

### Recursive DNS Resolver

A recursive resolver receives DNS requests from clients and works to find the requested DNS information. It may return an answer from its cache or contact other DNS servers to obtain the answer.

### Root DNS Server

A root DNS server helps direct DNS queries to the appropriate top-level domain servers. It does not normally provide the final IP address for a website.

### Top-Level Domain (TLD) DNS Server

A TLD server provides information about the authoritative DNS servers responsible for a domain under a particular extension, such as `.com`, `.org`, or `.ng`.

### Authoritative DNS Server

An authoritative DNS server holds DNS information for a domain and can provide the requested record, such as an A or AAAA record.

## 4. How DNS Resolution Works

Consider a user trying to access `www.example.com`.

1. The user enters the domain name into a browser.
2. The device checks whether it can use an existing DNS record from its cache.
3. If it cannot use a cached answer, it sends a DNS request to its configured resolver.
4. If the resolver does not already have a valid cached answer, it may query a root server.
5. The root server directs the resolver toward the appropriate TLD server.
6. The TLD server identifies the authoritative DNS server for the domain.
7. The resolver queries the authoritative server for the requested record.
8. The resolver returns the answer to the client and may cache it according to its TTL.
9. The device uses the returned IP address to connect to the website.

This is a simplified example. The exact process can vary depending on caching, DNS configuration, and the records involved.

## 5. Recursive and Iterative Queries

### Recursive Query

In a recursive query, the client asks the DNS resolver to obtain a final answer or return an error. The resolver handles the work required to find that answer.

### Iterative Query

In an iterative query, a DNS server can respond with the best information it has, such as a referral to another DNS server. The resolver can then contact that server to continue the resolution process.

Recursive queries are common between client devices and their configured DNS resolvers. Resolvers commonly use iterative queries when working through the DNS hierarchy.

## 6. DNS Records Involved in Resolution

| Record | Purpose |
|---|---|
| A | Maps a domain name to an IPv4 address. |
| AAAA | Maps a domain name to an IPv6 address. |
| CNAME | Identifies another domain name as an alias. |
| MX | Identifies mail servers for a domain. |
| NS | Identifies authoritative name servers for a domain. |

The record returned depends on the type of DNS information requested.

## 7. How DNS Caching Affects Resolution

DNS caching stores DNS records temporarily so that they can be reused while valid.

When a resolver has a valid cached record, it may answer a request without contacting other DNS servers. This can reduce lookup time and network traffic.

Caching can occur at different levels, including the operating system, browser, and recursive resolver.

The Time to Live (TTL) indicates how long a DNS record may remain cached before it should be refreshed. A record's TTL is different from the time taken to receive a DNS response.

## 8. Common DNS Resolution Problems

### DNS Server Unavailable

The configured resolver may be unreachable or temporarily unable to respond.

### Incorrect DNS Records

Incorrect or outdated DNS records may direct clients to an unintended IP address or prevent a domain from resolving correctly.

### DNS Timeout

A timeout occurs when a DNS response is not received within the allowed period. Possible causes include network problems, an unavailable resolver, packet loss, or filtering.

### DNS Resolution Failure

A domain may fail to resolve because the requested record does not exist, the DNS configuration is incorrect, or the resolution process encounters another error.

### Slow DNS Resolution

Slow resolution can result from network delays, resolver performance issues, retries, or the need to obtain an answer from other DNS servers.

## 9. Relevance to Security Operations Centre (SOC) Analysis

DNS is useful in security monitoring because devices frequently make DNS requests when accessing websites, applications, and external services.

A SOC analyst may investigate DNS activity to identify:

- Requests to suspicious or unusual domains.
- Repeated DNS requests to a particular destination.
- Unusual volumes of DNS traffic.
- DNS queries associated with potentially malicious activity.
- Unexpected changes in DNS resolution behaviour.

DNS activity should be assessed alongside other evidence, such as endpoint alerts, firewall logs, network traffic, and threat intelligence. A suspicious-looking domain or unusual query alone does not confirm that an attack has occurred.

## 10. Conclusion

DNS resolution enables devices to translate domain names into the information needed to communicate across networks. Understanding DNS clients, recursive resolvers, root servers, TLD servers, authoritative servers, and caching provides a foundation for troubleshooting network problems and investigating DNS activity in a SOC environment.

