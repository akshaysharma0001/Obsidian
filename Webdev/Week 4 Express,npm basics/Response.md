The response represents what the server returns you `in response` to the request.
It could be ->
1. Plaintext data - Not used as often
2. HTML - If it is a website
3. JSON Data - If you want to fetch some data (user details, list of todos…)

### JSON
**JSON** stands for **JavaScript Object Notation**. It is a lightweight, text-based format used for data interchange

```javascript
{
  "name": "John Doe",
  "age": 30,
  "isEmployed": true,
  "address": {
    "street": "123 Main St",
    "city": "Anytown"
  },
  "phoneNumbers": ["123-456-7890", "987-654-3210"]
}
```

# Ports
**Port 80**: [HTTP (Hypertext Transfer Protocol)](https://www.geeksforgeeks.org/computer-networks/what-is-ports-in-networking/) for unencrypted web pages.
**Port 443**: HTTPS (Hypertext Transfer Protocol Secure) for encrypted, secure web traffic.
**Port 8080 / 8443**: Alternative or proxy ports used for web applications
**Port 20**: [FTP (File Transfer Protocol)](https://en.wikipedia.org/wiki/List_of_TCP_and_UDP_port_numbers) data transfer.
**Port 25**: [SMTP (Simple Mail Transfer Protocol)](https://chadura.com/blogs/introduction-to-ports-in-operating-systems/) for routing mail between servers.
**Port 53**: DNS (Domain Name System) for translating human-readable web addresses into machine-readable IP numbers.
**Port 445**: SMB (Server Message Block) for sharing files and printers locally on Windows networks.