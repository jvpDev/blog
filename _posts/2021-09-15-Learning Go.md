---
title: Leraning Go
tags: learning coding developing go golang
---

I looked at a Golang code and was curious on how does this language works. The concurency mechanism seems interesting. Much more than Python.
I will try to implement PoW with concurency in Golang in a few days. Maybe its a fun exercise to try to implement it in many diferent languages and maybe after put it into github.

```go
package main

import (
  "fmt"
  "crypto/sha256"
  "strings"
)


func proofOfWork( data string, difficulty string ) (int64,string) {
  nonce := int64( 0 )
  for {
    hash := calcHash( intToStr(nonce) + data )
    if strings.HasPrefix( hash, difficulty ) {
        return nonce,hash 
    } else {
        nonce += 1           
    }
  }
}
```