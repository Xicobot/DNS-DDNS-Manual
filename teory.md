# Internet Domain Name System (DNS)

The Internet Domain Name System (DNS) is a specific implementation of the concept of a name server, optimized for the conditions of the Internet network. This implementation covers three main requirements:

- The need for a hierarchy of names.
- The need for load balancing between name servers.
- The need to delegate the administration of name servers.

DNS is implemented through a tree structure, which is hierarchical. At the top level, there is the root node, followed by top-level domain (TLD) nodes, then second-level domain (SLD) nodes, and finally, an indefinite number of lower-level nodes. All of these levels are separated by a dot when written.

## Types of TLDs

TLDs are divided into two types:

1. **Generic Domains (Generic TLD, gTLD):** .com, .net, .org, etc.
2. **Geographic Domains or Country Code Domains (Country Code TLD, ccTLD):** .es, .fr, .uk, etc. (Always with two characters).

![imagen](https://www.fpgenred.es/DNS/jerarquia.png)

# Fully Qualified Domain Name (FQDN)

Names such as www.example.com, www.informatics.example.com, etc., are referred to as Fully Qualified Domain Names (FQDN). An FQDN is simply a name that includes both the hostname and the domain name of that host. Technically, an FQDN should end with a dot (www.example.com.), referencing the root node, but it is common practice to omit it.

The maximum allowed length for an FQDN (RFC 1035, RFC 1123, RFC 2181) is 255 characters, with an additional restriction of 63 bytes per label within a domain name. FQDN labels are restricted to a limited character set: letters A-Z, digits, and the hyphen (-), and they are case-insensitive. Since October 2007, the .es domain allows special characters, such as ñ, ç, or accented vowels like á, é, í, ó, ú, and ü.

The implementation of the Internet DNS is carried out through a series of name servers (running DNS software), and the responsibility for the proper functioning of these servers lies with the authoritative entities of the domain names.

![imagen2](https://www.fpgenred.es/DNS/DelegacionDominios.png)

# TLD Servers (gTLD and ccTLD)

TLD servers (gTLD and ccTLD) are managed by a variety of organizations under agreements with ICANN.

The organizations to which domain names have been delegated are responsible for the servers that implement these domains. Therefore, they can have their own DNS servers, at least two for security, and manage them. However, they may also choose to delegate the responsibility for server management to an ISP, a web hosting company, or a register domain name.

The process of querying the servers can be summarized in the following steps:

1. A user's device makes a query for the name `hola.example.com` to the DNS server it is configured with, and normally, it will not have an authoritative answer, so it will query one of the 13 root DNS servers.
2. The root server responds with a reference to the TLD server(s) for the query (.com).
3. The user's device sends the query (`hola.example.com`) to the TLD server.
4. The TLD server responds with a reference to the domain name server for the query (.example.com).
5. The user's device sends the query (`hola.example.com`) to the .example.com domain server.
6. The domain server sends the user’s device an authoritative response to the query.

These steps can be seen in the following image:
![imagen3](https://www.fpgenred.es/DNS/Consulta.png)

# DNS Query Response Process and Caching

In the DNS query response process, the various caches involved are very important as they significantly reduce response time. DNS caches are temporary storage for the most recent answers to DNS queries; when a query is made, the first step is to check if the response is in the cache, and if it is, the response is provided directly.

The responses stored in caches are assigned a time, known as Time To Live (TTL). Once this time has passed and the response is not used again, it is removed from the cache, thus freeing up space to store other responses.

![imagen](https://www.fpgenred.es/DNS/ProcedimientoConsultaConCache.png)

# Zone and Zone File in DNS

Two very important concepts, already mentioned, are the zone and the zone file that describes it, converting the domain name into operational entities such as hosts, mail servers, services, and other characteristics used by DNS software. Subdomains delegated by the domain owner can also be described using a zone file.

The zone file describes, using the so-called Resource Records (RR), the part of the domain name that is managed by DNS software, that is, the zone. The format of these files and their RRs are standardized in RFC 1035, allowing them to be exchanged between different standard DNS software. These files typically contain the following RRs:

- **SOA RR (Start Of Authority)**: This record presents the properties of the zone and is mandatory.
- **A and AAAA RRs**: These specify the hosts (computers) within the zone.
- Data describing global information about the zone, such as:
  - Mail servers for the domain (MX RR).
  - Authoritative name servers for the domain (NS RR).
- In the case of delegated subdomains, the name servers responsible for the subdomain (NS RR).
- For delegated subdomains, a record allowing the domain server to reach the subdomain server (A or AAAA RR).

# DNS Authoritative Server and Resolver

Most of the work of an authoritative DNS server is dedicated to responding to DNS queries from programs called resolvers. The resolver on a PC is a library installed on the user's computer that handles translating user program DNS queries into the DNS protocol and then sending them out. 

The most common case is when a browser on a user's computer asks the following question: "What is the IP address of www.example.com?" and sends this request to the PC's resolver. The resolver will translate the query into the DNS protocol and send it to the resolver of the name server configured locally, either through DHCP or manually. From there, the query may proceed in several ways. In this case, it would be a recursive query, as shown in the following image:

## DNS Query Types

The DNS protocol defines three types of queries:

- Recursive
- Iterative or non-recursive
- Inverse
## 1. **Recursive Query**
In a **recursive query**, the DNS client (usually a resolver) expects the DNS server to take on the full responsibility of finding the requested information. If the server does not have the information in its cache, it will query other DNS servers on behalf of the client until the answer is found, or the query fails. 

- **Example**: A user queries their local DNS server for the IP address of `example.com`. If the local DNS server does not have this information cached, it will perform the necessary steps to find the authoritative server for the domain and return the final answer to the user.

- **Key point**: The client waits until it gets a final answer (either a result or an error), and the DNS server handles the entire process of querying other servers if needed.

## 2. **Iterative Query (Non-recursive Query)**
In an **iterative (non-recursive) query**, the DNS client makes the request to the DNS server, but the server responds with the best answer it can. If the server doesn’t have the requested information, it will return a referral to another DNS server, which may have the information or be able to refer the client further down the line.

- **Example**: A user queries a DNS server for `example.com`. If the server does not have the direct answer, it will provide the IP address of the next DNS server that may know the answer (for example, the authoritative server for `.com`). The client then sends the query to the next server in the referral chain.

- **Key point**: The DNS client must then continue the process itself by querying other DNS servers directly. The client doesn’t wait for the server to complete the full query chain.

## 3. **Inverse Query**
An **inverse query** is a reverse DNS lookup. Instead of resolving a domain name to an IP address (as is done in typical queries), an inverse query resolves an IP address to a domain name. 

- **Example**: Given an IP address like `192.168.1.1`, an inverse query might return the domain name associated with that IP address, such as `server.example.com`.

- **Key point**: This type of query is often used for diagnostic purposes (such as for reverse IP lookups) and is defined by querying the special `in-addr.arpa` domain (for IPv4 addresses) or `ip6.arpa` (for IPv6 addresses).

## Summary:
- **Recursive Query**: The server performs all the work and returns the final answer.
- **Iterative Query (Non-recursive)**: The server provides a referral to another server if it doesn't have the answer.
- **Inverse Query**: Resolves an IP address to a domain name (reverse lookup).
