# PhaseStream 1
The challenge was a repeating 5-byte string is the key to this XOR'ed string.
To solve, I brute-forced 5-byte strings until I found a match to the flag's prefix `CHTB{`.

Here is my code:
```python=
from string import ascii_lowercase
from itertools import product
s1 = '2e313f2702184c5a0b1e321205550e03261b094d5c171f56011904'

# only generate 5 bytes
for x in range(5, 6):
	for combo in product(ascii_lowercase, repeat=x):
		# xor the two strings as bytes
		cur = bytes([a ^ b for a, b in zip(bytes(''.join(combo), 'utf-8'), bytes.fromhex(s1))])
		# if found output the key and the flag
		if (cur == b'CHTB{'):
			key = ''.join(combo)
			print("Key found!", key)
			print("Flag:", bytes([a ^ b for a, b in zip(bytes(''.join(combo)*6, 'utf-8'), bytes.fromhex(s1))]).decode('utf-8'))
			exit()
```
Overall not to bad.