# DNS Troubleshooting

## 1. Introduction to DNS Troubleshooting

DNS (Domain Name System) troubleshooting involves identifying and resolving issues related to domain name resolution.

DNS translates human-readable domain names, such as `www.google.com`, into IP addresses that computers use to communicate over a network.

When DNS fails, users may experience difficulty accessing websites, applications, email services, or other network resources.

DNS troubleshooting helps determine whether a connectivity problem is caused by DNS configuration, DNS servers, network connectivity, or incorrect DNS records.

## 2. Common DNS Problems

### DNS Server Not Responding

This occurs when a device cannot obtain a response from its configured DNS server.

Possible causes include:

- DNS server downtime.
- Incorrect DNS server configuration.
- Network connectivity problems.
- Firewall rules blocking DNS traffic.
- DNS service failure.

### Incorrect DNS Records

DNS records may contain incorrect or outdated information.

For example, an A record may point to the wrong IPv4 address, preventing users from reaching the intended server.

### DNS Cache Issues

A device may temporarily store outdated DNS information, causing it to connect to an old IP address.

Clearing the DNS cache can help when the cached information is incorrect.

### DNS Propagation Delays

When DNS records are changed, different recursive resolvers may continue returning previously cached values until their TTLs expire.

This can cause users in different locations to receive different DNS responses.

### DNS Resolution Failure

A DNS resolution failure occurs when a domain name cannot be successfully resolved.

Possible causes include incorrect domain spelling, missing records, DNS server problems, or connectivity issues.

## 3. Common DNS Error Messages

| Error | Meaning |
|---|---|
| DNS_PROBE_FINISHED_NXDOMAIN | The requested domain name does not exist according to the DNS response |
| DNS_PROBE_FINISHED_BAD_CONFIG | A DNS configuration or resolution problem was detected |
| DNS_PROBE_FINISHED_NO_INTERNET | The browser detected an internet connectivity problem |
| DNS Server Not Responding | The configured DNS resolver could not be reached or did not respond |
| DNS Request Timed Out | A DNS query did not receive a response within the expected time |

## 4. DNS Troubleshooting Commands

### ipconfig /all

```cmd
ipconfig /all
```

Displays detailed network configuration, including IP addresses, DNS servers, default gateways, and DHCP information.

**Use:** To identify incorrect DNS server settings or network configuration problems.

### nslookup

```cmd
nslookup google.com
```

Queries a DNS server to retrieve DNS information for a domain.

**Use:** To verify whether a domain resolves successfully.

A specific DNS resolver can also be queried:

```cmd
nslookup google.com 8.8.8.8
```

This queries Google's public DNS resolver.

### ping

```cmd
ping google.com
```

Tests whether a domain resolves to an IP address and whether the destination responds to ICMP echo requests.

A failed ping does not always indicate a DNS problem because some servers block ICMP traffic.

### ipconfig /displaydns

```cmd
ipconfig /displaydns
```

Displays records stored in the Windows DNS Client resolver cache.

**Use:** To inspect previously cached DNS records and their remaining TTL values.

### ipconfig /flushdns

```cmd
ipconfig /flushdns
```

Clears the Windows DNS resolver cache.

**Use:** To remove cached DNS information when investigating stale or incorrect records.

### tracert

```cmd
tracert google.com
```

Displays the network path that packets attempt to take to reach a destination.

**Use:** To investigate network routing and connectivity problems after DNS resolution.

### Resolve-DnsName

```powershell
Resolve-DnsName google.com
```

Performs DNS resolution using PowerShell.

A specific record type can be requested:

```powershell
Resolve-DnsName google.com -Type MX
```

**Use:** To investigate DNS records such as A, AAAA, MX, CNAME, and TXT.

## 5. Step-by-Step DNS Troubleshooting Process

### Step 1: Check Network Connectivity

Verify that the device has an active network connection.

```cmd
ipconfig
```

Check the IPv4 address, subnet mask, and default gateway.

### Step 2: Test IP Connectivity

```cmd
ping 1.1.1.1
```

This tests connectivity to Cloudflare's public DNS IP address without requiring domain name resolution.

If the ping fails, investigate network connectivity. However, ICMP filtering may also cause failure.

### Step 3: Test DNS Resolution

```cmd
nslookup google.com
```

If DNS resolution fails while IP connectivity works, investigate the configured DNS resolver and DNS settings.

### Step 4: Check DNS Server Configuration

```cmd
ipconfig /all
```

Identify the DNS servers configured on the device.

Check whether the DNS server addresses are correct and reachable.

### Step 5: Compare DNS Resolvers

```cmd
nslookup google.com 8.8.8.8
```

Compare the result with the response from the configured DNS resolver.

Different responses may be legitimate because of caching, geographic routing, load balancing, or DNS policies.

### Step 6: Inspect and Clear DNS Cache

```cmd
ipconfig /displaydns
```

If stale cached information is suspected, clear the cache:

```cmd
ipconfig /flushdns
```

Then repeat the DNS lookup to determine whether the problem persists.

## 6. Understanding DNS Response Codes

DNS response codes help identify the outcome of a DNS query.

| Response Code | Meaning |
|---|---|
| NOERROR | The DNS query was processed without a DNS protocol error |
| NXDOMAIN | The requested domain name does not exist |
| SERVFAIL | The DNS server could not complete the query |
| REFUSED | The DNS server refused to process the query |
| FORMERR | The DNS server could not interpret the query format |

A NOERROR response does not necessarily mean the requested record type exists. A domain may exist without having a record of the requested type.

## 7. DNS Troubleshooting in Cybersecurity

DNS troubleshooting is important in cybersecurity because DNS is used by legitimate applications and can also be abused by attackers.

SOC Analysts may investigate DNS activity to identify:

- Suspicious domain lookups.
- Malware command-and-control communication.
- DNS tunneling.
- DNS spoofing and cache poisoning.
- Unexpected DNS server changes.
- Connections to malicious domains.

### Example Investigation

A workstation repeatedly attempts to resolve an unfamiliar domain.

A SOC Analyst may investigate:

1. Which endpoint generated the DNS queries?
2. When did the DNS activity begin?
3. Which DNS server handled the requests?
4. What DNS records were returned?
5. Is the domain associated with known malicious activity?
6. Did the endpoint establish connections to the returned IP addresses?

DNS logs should be correlated with firewall logs, endpoint telemetry, SIEM alerts, and threat intelligence before determining whether the activity is malicious.

## 8. DNS Troubleshooting Best Practices

- Verify network connectivity before assuming DNS is responsible.
- Confirm that the configured DNS servers are correct.
- Compare DNS responses from different resolvers when appropriate.
- Check for outdated or incorrect DNS records.
- Review TTL values when investigating caching issues.
- Avoid changing DNS configurations without authorization.
- Document troubleshooting commands and their results.
- Correlate DNS findings with other security logs.

## 9. Conclusion

DNS troubleshooting is an essential networking skill that helps identify and resolve domain resolution problems.

Understanding DNS commands, DNS response codes, caching behaviour, and network connectivity makes it easier to distinguish DNS failures from other network issues.

For cybersecurity investigations, DNS troubleshooting also helps SOC Analysts identify suspicious domain activity, investigate potential threats, and understand how endpoints communicate with external services.
