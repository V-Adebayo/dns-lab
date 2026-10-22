# DNS Records Investigation and Packet Analysis

## Overview

In this project, I investigated A, AAAA, CNAME, MX and NS records
using nslookup. I also captured DNS traffic in Wireshark to examine
queries, responses and DNS activity during a browser visit.

## Tools Used

- Windows Command Prompt
- nslookup
- Wireshark
- Web browser

## 1. DNS Record Investigation

### A Record — IPv4 Address

An A record maps a domain name to an IPv4 address.

Command used:

    nslookup -type=A example.com

The lookup returned:

- 104.20.23.154
- 172.66.147.243

### AAAA Record — IPv6 Address

An AAAA record maps a domain name to an IPv6 address.

Command used:

    nslookup -type=AAAA example.com

The lookup returned:

- 2606:4700:10::ac42:93f3
- 2606:4700:10::6814:179a

The screenshot below shows both the A and AAAA lookups.

![A and AAAA lookup results](projects/dns-a-aaaa.png)

### CNAME Record — Domain Alias

A CNAME record points an alias to another domain name.

I first ran:

    nslookup -type=CNAME www.example.com

The output displayed SOA information rather than a CNAME answer.
I then ran:

    nslookup -type=CNAME www.github.com

Result:

    www.github.com canonical name = github.com

This showed that www.github.com was an alias for github.com.
A CNAME points to another domain name rather than directly to
an IP address.

### MX Record — Email Routing

An MX record specifies the mail servers for a domain.
Lower preference values indicate higher priority.

Command used:

    nslookup -type=MX example.com

Result:

    MX preference = 0, mail exchanger = (root)

This is a Null MX record, indicating that example.com does not
accept email.

The screenshot below shows the original CNAME attempt and the MX result.

![Original CNAME query and MX lookup result](projects/dns-cname-mx.png)

### NS Record — Authoritative Name Servers

NS records identify the authoritative name servers for a domain.

Command used:

    nslookup -type=NS example.com

The lookup returned:

- hera.ns.cloudflare.com
- elliott.ns.cloudflare.com

The output stated "Non-authoritative answer" because my resolver
supplied the result rather than a direct query to an authoritative server.

## 2. DNS Resolver Observations

During the initial nslookup tests, the displayed DNS server address was:

    fe80::1

This is an IPv6 link-local address.

During the later Wireshark capture, I observed these addresses:

| Role | IP address |
|---|---|
| Client | 172.20.10.2 |
| DNS resolver | 172.20.10.1 |

These observations came from different stages of the exercise.
They do not identify the resolver's upstream DNS service.

## 3. DNS Query and Response Analysis

I started a Wireshark capture on my active Wi-Fi interface and ran:

    nslookup -type=A iana.org

I applied this display filter:

    dns.qry.name == "iana.org"

I inspected the query and its corresponding response.

| Field | Observed value |
|---|---|
| Query packet | 3057 |
| Response packet | 3067 |
| Transaction ID | 0x0002 |
| Domain queried | iana.org |
| Record type | A |
| Returned IPv4 address | 192.0.43.8 |
| Response code | No error (0) |
| TTL | 4502 seconds |
| Query-to-response time | 251.305 milliseconds |

The matching transaction ID and Wireshark's request reference helped
link the response to the query.

### Response Flags

The response showed that:

- It was a standard query response.
- The server reported no DNS error.
- The responding server was not authoritative for the domain.
- Recursion was requested and available.
- The response was not truncated.

### TTL and Response Time

The returned A record had a TTL of 4502 seconds, equivalent to
1 hour, 15 minutes and 2 seconds. This was the cache lifetime
supplied in that response.

The query-to-response time was 251.305 milliseconds. This was
one observed lookup, not an overall DNS performance measurement.

## 4. DNS Activity During a Browser Visit

While Wireshark was capturing, I visited:

    https://www.iana.org

I used this display filter:

    dns.qry.name == "www.iana.org"

New A and HTTPS DNS queries appeared during the visit.

- A queries requested IPv4 addresses.
- HTTPS DNS queries requested HTTPS service information.
  These queries were separate from the encrypted webpage traffic.

The client at 172.20.10.2 sent queries to the resolver at
172.20.10.1, which returned the responses.

![DNS queries and responses during the browser visit](projects/browser-dns-queries.png)

### Browser DNS Response

I inspected response packet 17300, with transaction ID 0x8a3c.

It reported "No error" and contained three answer records:

| Record type | Name | Returned value |
|---|---|---|
| CNAME | www.iana.org | www.iana.org.cdn.cloudflare.net |
| A | www.iana.org.cdn.cloudflare.net | 104.18.25.232 |
| A | www.iana.org.cdn.cloudflare.net | 104.18.24.232 |

The CNAME pointed www.iana.org to another domain name.
The two A records supplied IPv4 addresses for that target.

The CNAME had a TTL of 1564 seconds. The A record for
104.18.25.232 had a TTL of 172 seconds. This showed that
records within the same response can have different cache lifetimes.

![Browser DNS response showing CNAME and A records](projects/browser-dns-answers.png)

## 5. Evidence Collected

The repository includes four screenshots:

- A and AAAA lookup results
- Original CNAME attempt and MX lookup result
- DNS queries and responses during the browser visit
- Browser DNS answers showing a CNAME and two A records

The successful www.github.com CNAME lookup, NS lookup and initial
iana.org packet analysis are documented in the written findings.
Separate screenshots for those results are not included here.

I also saved a local packet capture named:

    dns-investigation.pcapng

I exported the displayed packets using this filter:

    dns.qry.name == "iana.org" || dns.qry.name == "www.iana.org"

## Skills Developed

- Querying and interpreting DNS records
- Using Windows networking commands
- Capturing and filtering traffic in Wireshark
- Matching DNS queries with responses
- Reading response flags and TTL values
- Following CNAME answers to IPv4 addresses
- Documenting observations from network traffic

## Limitations

- The results reflect the time and network used for testing.
- DNS answers and TTL values can change.
- A local capture does not reveal the resolver's complete upstream path.
- Browser activity was associated with the observed traffic by timing
  and domain name. The capture alone does not identify the process.
- Successful DNS resolution does not prove that DNS is secure.
- This was a DNS investigation and packet analysis exercise, not a
  penetration test. No DNS vulnerability was established.

## Conclusion

This project helped me understand the purpose of different DNS records
and connect command-line lookups with captured network traffic.

I practised matching queries and responses, interpreting DNS flags
and TTLs, and examining how a browser lookup can return a CNAME
alongside its associated IPv4 addresses.
