---
title: rust common cheat sheet
author: LMJW
tags: [rust, CS]
---

- `String.push(char)` takes char
- `Vec` has `reverse` method where as iterator has `rev` method
- `String` and `&str` both have `chars` method that convert string to `Char`
- `String` and `&str` can be `split_whitespace`
    - they also can call `split(pat: P)` e.g `split(' ')` is equivalent to split by whitespace
- NOTE: in rust `usize -1` could overflow to max, this might causing algorithm not working properly
