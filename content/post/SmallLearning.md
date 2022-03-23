---
title: common knowledge cheat sheet
author: LMJW
tags: [rust, CS]
---

## Rust 

- `String.push(char)` takes char
- `Vec` has `reverse` method where as iterator has `rev` method
- `String` and `&str` both have `chars` method that convert string to `Char`
- `String` and `&str` can be `split_whitespace`
    - they also can call `split(pat: P)` e.g `split(' ')` is equivalent to split
      by whitespace
- NOTE: in rust `usize -1` could overflow to max, this might causing algorithm
  not working properly
- If we are dealing with linked list with `Option<Box<Node>>`, we can
  potentially using `Box::clone()` to overcome some limitation due to borrow
  checker. `Box::clone` should be relatively not expensive as mentioned in [rust
  book](https://doc.rust-lang.org/rust-by-example/std/box.html): `a box is a
  smart pointer to a heap allocated value of T`.

## Docker
- Docker can be run with `--network=host` on windows, but as stated by
  [reference](https://docs.docker.com/network/host/), quote:
  > The host networking driver only works on Linux hosts, and is not supported
  > on Docker Desktop for Mac, Docker Desktop for Windows, or Docker EE for
  > Windows Server. 
  
  So, when you notice something wired, like cannot ping docker exported port
  from windows/wsl2. check if your docker container is launched using
  `--network=host`.