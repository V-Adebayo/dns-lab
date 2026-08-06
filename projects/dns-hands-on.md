# DNS Browser Lab

## Overview

This lab demonstrates how the Domain Name System (DNS) works by observing how a web browser resolves a domain name into an IP address and communicates with a web server over HTTPS. The lab also explores browser Developer Tools to inspect network requests and understand the client-server communication process.

---

## Objectives

- Understand the role of DNS in web browsing.
- Resolve a domain name to an IP address using `nslookup`.
- Observe how browsers communicate with web servers.
- Inspect HTTP requests using the browser's Network tab.
- Verify that a website uses HTTPS.

---

## Tools Used

- Google Chrome
- macOS Terminal
- `nslookup`
- Browser Developer Tools (Network Tab)
- GitHub
- Markdown

---

## Website Tested

**Domain Name:** `google.com`

---

## Lab Procedure

### Step 1: Access the Website

I visited:

```
https://www.google.com
```

### Observations

- Website loaded successfully.
- HTTPS was used.
- Domain name: `google.com`

---

### Step 2: Resolve the Domain Name

I used the following command:

```bash
nslookup google.com
```

### Result

The command successfully resolved the domain name to an IP address.

**Purpose:**

DNS translates human-readable domain names into IP addresses that computers use to communicate over the internet.

---

### Step 3: Visit the IP Address

After obtaining the IP address using `nslookup`, I entered the IP address directly into the browser.

### Observations

- The webpage loaded successfully.
- No certificate warning was displayed.
- The browser did not redirect to another page.

---

### Step 4: Inspect Browser Network Requests

I opened Chrome Developer Tools and selected the **Network** tab before refreshing the webpage.

### Observations

- The browser generated multiple network requests to load the webpage.
- Most requests returned a **200 OK** status code.
- Some requests returned **204 No Content**.
- The browser downloaded different resource types including:
  - HTML documents
  - JavaScript files
  - Images
  - Fonts
  - XHR (AJAX) requests
- Several resources were loaded from the browser's **memory cache** and **disk cache**, improving page load performance.

---

### Step 5: HTTPS Verification

The website loaded using HTTPS.

### Observations

- The browser displayed no security warnings.
- The connection was encrypted using HTTPS.
- This indicates that the website presented a trusted TLS/SSL certificate.

---

## Understanding the 200 OK Status Code

During the Network analysis, most requests returned the status code **200 OK**.

This means:

- The client (web browser) successfully sent a request.
- The server successfully received the request.
- The server processed the request.
- The requested resource was successfully returned.

### Client-Server Example

```
Client (Browser)
       │
       │ GET https://www.google.com
       ▼
Google Web Server
       │
       │ HTTP 200 OK
       ▼
Returns the Google homepage
```

This demonstrates the **client-server model**, where the browser (client) requests resources and the web server responds with the requested data.

---

## Key Concepts Learned

### DNS

DNS (Domain Name System) translates domain names into IP addresses so browsers can locate web servers.

### HTTPS

HTTPS encrypts communication between the client and the server, protecting data from interception.

### Browser Developer Tools

The Network tab allows users to inspect:

- HTTP requests
- HTTP responses
- Status codes
- Resource types
- Loading times
- Network activity

This tool is widely used by web developers, network engineers, and cybersecurity analysts.

---

## Conclusion

This lab demonstrated how DNS enables users to access websites using domain names instead of IP addresses. By using `nslookup`, inspecting browser network requests, and observing HTTPS communication, I gained practical experience with DNS resolution, the client-server model, HTTP status codes, and browser developer tools.

---

## Screenshots

Include the following screenshots in the `screenshots` folder:

- `browser-homepage.png`
- `nslookup.png`
- `network-tab.png`
- `ip-address-test.png`

---

## Skills Demonstrated

- DNS Fundamentals
- Browser Developer Tools
- HTTP/HTTPS
- Client-Server Communication
- Network Troubleshooting
- Technical Documentation
- Markdown
- GitHub Documentation
