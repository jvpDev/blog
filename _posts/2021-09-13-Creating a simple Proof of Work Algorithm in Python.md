---
title: Creating a simple Proof of Work Algorithm in Python
tags: learning coding developing crypto blockchain proofOfWork
---

So, I've been interested in crypto/blockchain these days and today I decided to implement a simple Proof of Work algorithm, since this is the most popular consensus algorithm used in blockchains.

To keep as simple as possible, I chose sha256 as the hash function. The difficulty level corresponds to the number of zeroes that the start of the hash must have, so increasing it will make the algorithm take a longer time to find a valid hash. Difficulty level above 4 can be very slow, so watch out.

The solution attempts are made with a nonce value that is appended to the string. The appended string is hashed and it's evaluated. If is valid, you found your hash! If not valid, the nonce value is incremented, repeating the process.


All this, of course, implemented in python.

```python
import hashlib
import time

def proof_of_work(data, level):
    nonce = 0
    start = time.time()
    start_of_hash = '0' * level
    while True:
        data = f'{data}{nonce}'
        h = hashlib.sha256(bytes(data, encoding='utf8')).hexdigest()
        if h[0:level] == start_of_hash:
            time_elapsed = time.time() - start
            print('Hash Found!')
            print(f'Hash: {h}')
            print(f'Nonce: {nonce}')
            print(f'Time elapsed: {time_elapsed} seconds')
            return
        nonce += 1
```

```python
proof_of_work("any string that you want", 3)
```

```
Hash Found!
Hash: 000f226e1a0753df9f371c9100add3260e4549785f789b862a280c099919f6b1
Nonce: 2180
Time elapsed: 0.06898880004882812 seconds
```
