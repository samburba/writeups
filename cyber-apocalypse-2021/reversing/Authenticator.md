# Authenticator
The program asks for a "Alien ID":
```bash=
$ ./authenticator

Authentication System 👽

Please enter your credentials to continue.

Alien ID: test
Access Denied!
```
Looking into the file we see its not stripped:
```bash=
$ file authenticator
authenticator: ELF 64-bit LSB shared object, x86-64, version 1 (SYSV), dynamically linked, interpreter /lib64/ld-linux-x86-64.so.2, for GNU/Linux 3.2.0, BuildID[sha1]=66286657ca5a06147189b419238b2971b11c72db, not stripped
```
We can get a lot of useful information from the binary, like function names.
Lets try a couple "low-hanging fruit" of reversing (`string`, `ltrace`, `strace`).

 `ltrace` gives us the alien id, `11337`:
```bash=
strcmp("test\n", "11337\n")                                              
```
With the correct id, it now prompts for the pin. `string`,`ltrace`,`strace` doesn't help.

Opening it up in Ghidra, found the `checkpin` function:
```c=
undefined8 checkpin(char *param_1)

{
  size_t sVar1;
  int local_24;
  
  local_24 = 0;
  while( true ) {
    sVar1 = strlen(param_1);
    if (sVar1 - 1 <= (ulong)(long)local_24) {
      return 0;
    }
    if ((byte)("}a:Vh|}a:g}8j=}89gV<p<}:dV8<Vg9}V<9V<:j|{:"[local_24] ^ 9U) != param_1[local_24])
    break;
    local_24 = local_24 + 1;
  }
  return 1;
}
```

Clean up a little:
```c=
undefined8 checkpin(char *pin)

{
  size_t pin_len;
  int i;
  
  i = 0;
  while( true ) {
    pin_len = strlen(pin);
    if (pin_len - 1 <= (ulong)(long)i) {
      return 0;
    }
    if ((byte)("}a:Vh|}a:g}8j=}89gV<p<}:dV8<Vg9}V<9V<:j|{:"[i] ^ 9U) != pin[i]) break;
    i = i + 1;
  }
  return 1;
}
```

Looks like the function tests the inputed pin by XORing character by character to `9`, and comparing it to the its relative character in
`}a:Vh|}a:g}8j=}89gV<p<}:dV8<Vg9}V<9V<:j|{:`

Simple Python script to decode it:
```python=
s = '}a:Vh|}a:g}8j=}89gV<p<}:dV8<Vg9}V<9V<:j|{:'

[print(chr(ord(i) ^ 9), end='') for i in s]
print()
```

and we get the pin: `th3_auth3nt1c4t10n_5y5t3m_15_n0t_50_53cur3`, which is also the flag: `CHTB{th3_auth3nt1c4t10n_5y5t3m_15_n0t_50_53cur3}`.