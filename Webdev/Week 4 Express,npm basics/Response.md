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