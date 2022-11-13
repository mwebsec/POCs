
### Stored XSS #1

https://datamost.com/<USERNAME>/lists


- Vulnerable parameter "Name"

To Replicate Findings:
`
1. Login to account
2. Go to Lists page
3. Select New List
4. Select "Name" 
4. Add XSS payload "><script>alert(document.cookie)</script>`
