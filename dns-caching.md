# DNS Caching

## 1. Introduction to DNS Caching

DNS (Domain Name System) caching is the process of temporarily storing DNS lookup results so that previously resolved domain names can be accessed without performing a complete DNS lookup again.

When a user visits a website such as `www.google.com`, the device needs to resolve the domain name to an IP address. After successful resolution, the DNS result may be stored in a cache for future use.

DNS caching helps to:

- Reduce DNS lookup time.
- Improve website loading speed.
- Reduce unnecessary DNS queries.
- Minimize network traffic.
- Reduce the workload on DNS servers.

## 2. How DNS Caching Works

When a user enters a domain name in a browser, the following process may occur:

1. The browser checks whether it has a valid cached DNS result.
2. If no valid result exists, the operating system's DNS resolution process is used.
3. The operating system may check its local DNS cache.
4. If the record is unavailable, a query is sent to a configured DNS resolver.
5. The recursive DNS resolver checks its own cache.
6. If necessary, the resolver queries other DNS servers to obtain the answer.
7. The resolved IP address is returned to the requesting device.
8. The DNS response may be cached for future requests.

The exact process depends on the browser, operating system and DNS configuration.

### Example

A user visits `www.example.com`.

The DNS resolver returns an IP address such as `93.184.215.14` (illustrative).

The result is cached.

When the user visits the same domain again while the cached record remains valid, the stored DNS information can be reused.

## 3. DNS Cache Hit and Cache Miss

### DNS Cache Hit

A DNS cache hit occurs when the requested DNS record is already stored in the cache and has not expired.

**Example:** A user visits a website that was recently accessed. The system retrieves the DNS record from its cache.

### DNS Cache Miss

A DNS cache miss occurs when the requested DNS record is unavailable or has expired.

**Example:** A user visits a new website, and the resolver must obtain the DNS record from another DNS server.

## 4. Types of DNS Caching

### Browser DNS Cache

Web browsers may temporarily store DNS resolution results to improve browsing performance.

### Operating System DNS Cache

Operating systems such as Windows maintain DNS caches that can be used by applications.

### Recursive DNS Resolver Cache

Recursive DNS resolvers store DNS responses to serve future queries efficiently.

Examples of public DNS resolvers include:

- Google Public DNS: `8.8.8.8`
- Cloudflare DNS: `1.1.1.1`

## 5. DNS Time to Live (TTL)

TTL stands for Time to Live.

TTL specifies how long a DNS record may remain cached before it needs to be refreshed.

### Example

```text
example.com.  300  IN  A  93.184.215.14
```

Explanation:

- `example.com`: Domain name
- `300`: TTL in seconds
- `IN`: Internet class
- `A`: IPv4 address record
- `93.184.215.14`: Illustrative IPv4 address

A TTL of 300 seconds means the DNS record can be cached for approximately five minutes.

After the TTL expires, a fresh DNS resolution is normally required before the record can be reused.

### Why TTL Matters

A shorter TTL allows DNS changes to be reflected more quickly but may increase DNS queries.

A longer TTL reduces DNS queries but can cause older DNS information to remain cached for longer.

## 6. DNS Cache Poisoning

DNS cache poisoning is an attack in which incorrect or malicious DNS information is introduced into a DNS resolver's cache.

An attacker may attempt to redirect users from a legitimate website to a malicious destination.

### Example

1. A user attempts to visit `www.mybank.com`.
2. An attacker attempts to inject a forged DNS response.
3. A vulnerable DNS resolver accepts the forged response.
4. The malicious IP address is stored in the cache.
5. The user may be directed to an attacker-controlled website.

### Security Implications

DNS cache poisoning may lead to:

- Phishing attacks.
- Credential theft.
- Traffic redirection.
- Malware distribution.
- Unauthorized access to sensitive information.

### Mitigation

1. Enable DNSSEC validation where supported.
2. Keep DNS resolver software updated.
3. Restrict unauthorized DNS configuration changes.
4. Monitor suspicious DNS responses and unexpected IP address changes.
5. Use trusted and properly configured DNS resolvers.

## 7. DNS Caching in Cybersecurity and SOC Analysis

DNS caching is important in cybersecurity because it affects how domain resolution activity appears in network logs.

A SOC Analyst investigating suspicious network connections should understand that a device may communicate with a domain's IP address without generating a new DNS query every time.

This can happen when the device uses a previously cached DNS result.

### Security Investigation Example

Suppose an endpoint communicates with a suspicious IP address.

A SOC Analyst may investigate:

- Which domain was associated with the IP address?
- Was the DNS response legitimate?
- Was the domain recently resolved?
- Does the IP address appear in threat intelligence?
- Are other endpoints communicating with the same destination?
- Is there evidence of DNS cache poisoning or malicious redirection?

DNS cache information should be correlated with endpoint telemetry, DNS logs, firewall logs and other available security data.

## 8. DNS Caching Commands

The following commands can be used during DNS caching investigations on Windows.

### View the Local DNS Cache

```powershell
ipconfig /displaydns
```

Displays DNS records stored in the Windows DNS Client resolver cache.

### Clear the DNS Cache

```powershell
ipconfig /flushdns
```

Clears the Windows DNS Client resolver cache.

### Perform a DNS Lookup

```powershell
nslookup example.com
```

Queries a DNS server for the specified domain.

Note: `nslookup` does not directly display the Windows DNS Client cache.

### Resolve a Domain Using PowerShell

```powershell
Resolve-DnsName example.com
```

Performs DNS name resolution and displays the result.

### View a Record's TTL

```powershell
Resolve-DnsName example.com -Type A
```

The returned information may include a `TTL` field, depending on the response.

## 9. Difference Between DNS Caching and DNS Resolution

| DNS Resolution | DNS Caching |
|---|---|
| Finds DNS information for a domain | Temporarily stores DNS results |
| May involve querying DNS servers | May reuse previously retrieved results |
| Obtains an IP address or other DNS data | Reduces repeated lookups |
| Provides the requested DNS answer | Improves lookup efficiency |

## 10. Key Takeaways

- DNS caching temporarily stores DNS lookup results.
- A cache hit occurs when a valid cached record is available.
- A cache miss occurs when a record is unavailable or expired.
- TTL controls how long DNS records may remain cached.
- DNS caching can occur in browsers, operating systems and recursive resolvers.
- DNS cache poisoning can redirect users to malicious destinations.
- DNSSEC validation helps protect against forged DNS information.
- SOC Analysts should consider cached DNS results when investigating suspicious network activity.
