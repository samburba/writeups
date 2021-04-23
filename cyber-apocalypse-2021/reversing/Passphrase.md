# Passphrase
Running the program gives a simple input:
```bash=
$ ./passphrase

Halt! ⛔
You do not look familiar..
Tell me the secret passphrase: test

Intruder alert! 🚨
```

If you get the passphrase wrong the program exits.

Checking the file type, we can see its an ELF 64-bit executable. Furthermore, it is not stripped:
```bash=
$ file passphrase
passphrase: ELF 64-bit LSB shared object, x86-64, version 1 (SYSV), dynamically linked, interpreter /lib64/ld-linux-x86-64.so.2, for GNU/Linux 3.2.0, BuildID[sha1]=60f6b6064d2e34a2b6a24dda9feb943b0b8c360f, not stripped
```

 `ltrace`  gives the flag  easily:
```bash=
strcmp("3xtr4t3rR3stR14L5_VS_hum4n5", "test")                           
```

If we submit that string, we get the flag:
```bash=
Halt! ⛔
You do not look familiar..
Tell me the secret passphrase: 3xtr4t3rR3stR14L5_VS_hum4n5
✔

Sorry for suspecting you, please transfer this important message to the chief: CHTB{3xtr4t3rR3stR14L5_VS_hum4n5}
```

The flag: `CHTB{3xtr4t3rR3stR14L5_VS_hum4n5}`
