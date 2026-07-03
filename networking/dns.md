# DNS (Domain Name System)

DNS translates domain names into IP addresses.

Example:

google.com → 142.250.x.x

Process:

1. Browser checks cache
2. OS checks local DNS cache
3. Query goes to recursive resolver
4. Resolver checks root server
5. Root points to TLD server (.com)
6. TLD points to authoritative server
7. Authoritative server returns IP
8. Browser connects to server

Common DNS record types:

A → IPv4 address  
AAAA → IPv6  
MX → Mail server  
TXT → Verification/security  
CNAME → Alias

Default port:

UDP 53  
TCP 53 for large transfers
