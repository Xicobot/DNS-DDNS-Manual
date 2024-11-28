The Domain Name System (DNS) is a fundamental component of the internet that translates human-readable domain names (e.g., www.example.com) into numerical IP addresses (e.g., 192.0.2.1), enabling devices to locate and communicate with each other across the network.

## How DNS Works:
1. User Request: When a user types a domain name into their browser, the request is sent to a DNS resolver, typically managed by the user’s internet service provider (ISP).
2. Recursive Query: The resolver checks its local cache for the IP address. If unavailable, it queries a root DNS server to identify which top-level domain (TLD) server (e.g., .com, .org) to contact.
3. TLD Server Response: The resolver then contacts the TLD server, which directs it to the authoritative DNS server for the specific domain.
4. Authoritative DNS Server: This server holds the DNS records for the requested domain and returns the corresponding IP address to the resolver.
5. Response to the Browser: The resolver sends the IP address back to the user’s browser, which uses it to establish a connection to the web server hosting the website.
6. Caching: To improve efficiency and reduce load, DNS responses are cached at various levels (e.g., by the resolver, the user’s device) for a specified time known as Time-to-Live (TTL).
7. This hierarchical process ensures that domain names are translated efficiently and correctly, facilitating seamless browsing and resource access on the internet.

In this repository i will give you the files so you can work with that without needing anything more.
You can find it [here](.conf)!
