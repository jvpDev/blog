---
title: Multiprocessing Proof of Work Algorithm in Python
tags: learning coding developing crypto blockchain proofOfWork multiprocessing
---

So, in the last post I implemented a simple Proof of Work algorithm in Python and I had never paid attention to python multiprocessing module, so I joined this two!

As the difficulty level of PoW rises, it take longer to find a solution. The intention is to have multiple workers trying to solve the job, arriving to a solution faster than a single worker.

It's my first time implementing a multiprocessing algorithm in python, so, as expected, I am not satisfied with the result. The performance was not significantly better, so it has a lot to improve.


```python
import time
import hashlib

from multiprocessing import Pool, cpu_count

LIMIT = 2e8

def proof_of_work(i, data, level):
    start_of_hash = '0' * level
    nonce = i * LIMIT + 1
    while nonce % LIMIT != 0:
        data = f'{data}{nonce}'
        h = hashlib.sha256(bytes(data, encoding='utf8')).hexdigest()
        if h[0:level] == start_of_hash:
            return h
        nonce += 1
    return 0


def quit(arg):
    print(f"Hash Found: {arg}")
    p.terminate()


if __name__ == "__main__":
    start = time.perf_counter_ns()

    ncpu = cpu_count()
    p = Pool(ncpu)
    for i in range(ncpu):
        p.apply_async(proof_of_work, args=(i, "any string", 6), callback=quit)
    p.close()
    p.join()
    time1 = time.perf_counter_ns() - start
    print(f"Time elapsed Mult: {time1}")

    start = time.perf_counter_ns()
    proof_of_work(0, "any string", 6)
    time2 = time.perf_counter_ns() - start
    print(f"Time elapsed Single: {time2}")
    print(time1/time2)

```
