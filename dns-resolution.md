# DNS Resolution

DNS resolution is the process of changing a domain name into an IP address.

For example:

```text
google.com → IP address
```

We use domain names because they are easier for humans to remember, but computers use IP addresses to communicate.

## How DNS Resolution Works

When I type a website like:

```text
www.google.com
```

into my browser, the following happens:

### 1. My computer checks its cache

First, my computer checks if it already knows the IP address of the website.

If it finds it, it can use the IP address directly.

If it does not find it, the DNS lookup continues.

### 2. The request goes to the Recursive DNS Resolver

My computer sends the DNS request to a **recursive resolver**.

The resolver's job is to find the IP address for me.

It first checks its own cache.

If it has the answer, it sends it back.

If it doesn't, it continues the lookup.

### 3. The Resolver asks the Root Server

The resolver contacts a **Root DNS Server**.

The root server does not usually give the IP address.

Instead, it tells the resolver where to find the server responsible for the domain extension.

For example:

```text
google.com
       ↑
      .com
```

The root server basically says:

> "Ask the .com TLD server."

### 4. The Resolver asks the TLD Server

The resolver then contacts the **.com TLD server**.

The TLD server knows which authoritative DNS server is responsible for the domain.

It tells the resolver:

> "Ask this authoritative DNS server."

### 5. The Resolver asks the Authoritative DNS Server

The resolver now contacts the **authoritative DNS server**.

This server contains the actual DNS records for the domain.

It can provide the IP address.

For example:

```text
google.com → 142.250.x.x
```

### 6. The Answer Goes Back

The authoritative server sends the IP address back to the recursive resolver.

Then the resolver sends the IP address back to my computer.

```text
Authoritative Server
        ↓
Recursive Resolver
        ↓
My Computer
```

My computer can now use the IP address to connect to the website.

## The Full Process

```text
My Computer
     ↓
Recursive Resolver
     ↓
Root DNS Server
     ↓
TLD Server (.com)
     ↓
Authoritative DNS Server
     ↓
IP Address
     ↓
My Computer
```

## Important Things I Understand

* **Client** → asks for the IP address.
* **Recursive Resolver** → does the lookup on behalf of the client.
* **Root Server** → directs the resolver to the correct TLD.
* **TLD Server** → directs the resolver to the authoritative server.
* **Authoritative Server** → gives the actual DNS record.

## DNS Caching

DNS also uses caching to make lookups faster.

For example, if the resolver already knows:

```text
google.com → IP address
```

it doesn't need to go through Root → TLD → Authoritative Server again.

It can just return the cached answer.

This is called a **cache hit**.

If the answer is not in the cache, it is a **cache miss**, and the resolver has to perform the lookup.

## Simple Way I Remember DNS Resolution

I remember it like this:

```text
Client asks:
"What is the IP?"

        ↓

Resolver:
"I will find it."

        ↓

Root:
"Ask the .com server."

        ↓

TLD:
"Ask this authoritative server."

        ↓

Authoritative:
"Here is the IP address."

        ↓

Resolver:
"Here is the IP address."

        ↓

Client:
"Now I can connect."
```

### In one sentence

**DNS resolution is how a domain name is converted into an IP address so the computer can connect to the correct server.**

