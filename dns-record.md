# DNS Records

## Overview

DNS (Domain Name System) records provide information about a domain and tell DNS servers how to handle requests for that domain.

Different DNS record types serve different purposes, such as directing traffic to a web server, identifying mail servers, or providing information about the domain.

---

## Common DNS Record Types

| Record    | Purpose                                                                     | Example                              |
| --------- | --------------------------------------------------------------------------- | ------------------------------------ |
| **A**     | Maps a domain name to an IPv4 address                                       | `example.com → 93.184.216.34`        |
| **AAAA**  | Maps a domain name to an IPv6 address                                       | `example.com → 2606:2800:220:1:...`  |
| **CNAME** | Creates an alias for another domain name                                    | `www.example.com → example.com`      |
| **MX**    | Specifies the mail servers responsible for receiving email                  | `example.com → mail.example.com`     |
| **NS**    | Identifies the authoritative name servers for a domain                      | `example.com → ns1.example.com`      |
| **TXT**   | Stores text information associated with a domain                            | SPF/DKIM verification                |
| **SOA**   | Contains administrative information about a DNS zone                        | Primary DNS server and serial number |
| **PTR**   | Maps an IP address back to a domain name (reverse DNS)                      | `93.184.216.34 → example.com`        |
| **SRV**   | Specifies the location of services such as VoIP or messaging                | `_sip._tcp.example.com`              |
| **CAA**   | Specifies which Certificate Authorities can issue certificates for a domain | `example.com → Let's Encrypt`        |

---

## 1. A Record

The **A (Address) record** maps a domain name to an **IPv4 address**.

### Example

```text
example.com → 93.184.216.34
```

### Query

```bash
nslookup example.com
```

or:

```bash
dig A example.com
```

---

## 2. AAAA Record

The **AAAA record** maps a domain name to an **IPv6 address**.

### Example

```text
example.com → 2606:2800:220:1:248:1893:25c8:1946
```

### Query

```bash
dig AAAA example.com
```

---

## 3. CNAME Record

A **CNAME (Canonical Name) record** creates an alias from one domain name to another.

### Example

```text
www.example.com → example.com
```

A CNAME does not normally point directly to an IP address. It points to another domain name.

### Query

```bash
dig CNAME www.example.com
```

---

## 4. MX Record

An **MX (Mail Exchange) record** specifies the mail servers responsible for receiving email for a domain.

### Example

```text
example.com → mail.example.com
```

MX records also have a **priority value**. A lower number represents a higher priority.

### Query

```bash
dig MX example.com
```

---

## 5. NS Record

An **NS (Name Server) record** identifies the authoritative DNS servers for a domain.

### Example

```text
example.com → ns1.example.com
example.com → ns2.example.com
```

### Query

```bash
dig NS example.com
```

---

## 6. TXT Record

A **TXT record** stores text associated with a domain.

TXT records are commonly used for:

* SPF email authentication
* Domain verification
* DKIM-related information
* DMARC-related configuration
* Other security and verification purposes

### Query

```bash
dig TXT example.com
```

---

## 7. SOA Record

The **SOA (Start of Authority) record** contains administrative information about a DNS zone.

It can contain information such as:

* Primary authoritative DNS server
* Responsible administrator
* Zone serial number
* Refresh interval
* Retry interval
* Expiration time
* Minimum/negative caching value

### Query

```bash
dig SOA example.com
```

---

## 8. PTR Record

A **PTR (Pointer) record** is used for **reverse DNS lookup**.

Instead of mapping a domain to an IP address, it maps an IP address back to a domain name.

### Forward DNS

```text
example.com → 93.184.216.34
```

### Reverse DNS

```text
93.184.216.34 → example.com
```

### Query

```bash
dig -x 93.184.216.34
```

---

## 9. SRV Record

An **SRV (Service) record** specifies the hostname and port where a particular service can be found.

It is commonly used for services such as:

* SIP/VoIP
* XMPP
* Other network services

### Example

```text
_sip._tcp.example.com
```

### Query

```bash
dig SRV _sip._tcp.example.com
```

---

## 10. CAA Record

A **CAA (Certification Authority Authorization) record** specifies which Certificate Authorities (CAs) are allowed to issue SSL/TLS certificates for a domain.

### Query

```bash
dig CAA example.com
```

---

## DNS Record Lookup

DNS records can be examined using command-line tools such as `dig` and `nslookup`.

### Using `dig`

```bash
dig A example.com
dig AAAA example.com
dig MX example.com
dig NS example.com
dig TXT example.com
```

### Using `nslookup`

```bash
nslookup example.com
```

To query a specific record type:

```bash
nslookup -type=MX example.com
```

---

## Key Takeaways

* **A** → Domain to IPv4
* **AAAA** → Domain to IPv6
* **CNAME** → Alias to another domain
* **MX** → Mail server
* **NS** → Authoritative name server
* **TXT** → Text, verification and security information
* **SOA** → DNS zone administrative information
* **PTR** → IP address to domain (reverse DNS)
* **SRV** → Service location
* **CAA** → Authorized Certificate Authorities

Understanding DNS records is important for troubleshooting **domain resolution, websites, email delivery, network services, and security configurations**.
