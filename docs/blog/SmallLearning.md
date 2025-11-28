---
title: 2022-03-22-common knowledge cheat sheet
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
- Check if key in HashMap, use `HashMap.contains_key(&key)`,
  `HashMap.remove(&key)` for delete, `HashMap.insert(key, val)` for insert key
- `*HashMap.entry(&key).or_insert(value) += 1` can be used as default map
- Double side queue: `VecDeque`, we can `push/pop_front` and `push/pop_back`
- `char` can be force convert to `u8` if we want: `'a' as u8` 
- `Rc<RefCell<T>>` can be get the internal `&mut T` by call `borrow_mut()`
- `&mut [T]` can call `&mut [T].clone/copy_from_slice(&[T])`, but their length
  needs to be the same
- `&[T]` has method `to_vec` that can clone it to a new vector
- `&mut [T]` has `rotate_left/right`  and `reverse` method
- `Vec<char>` can be collect into string using
  `x.chars().into_iter().collect<String>()`
- `char` can use `to_uppercase/lowercase()` convert to up/lower case characters.
- `char` has `is_ascii_alphabetic` method
- `slice` has method `.split_at(usize)` so the slice can be split into two parts
- `iter` has fold method, which allows we to fold iterator. e.g.
  ```rust
  vec![1,1,1,2,3].iter().fold(HashMap::new(), |mut map, i|{
    let val = map.entry(i).or_insert(0); *val += 1; map
  })`
  // equals to HashMap{1:3, 2:1, 3:1}
  ```






## Docker
- Docker can be run with `--network=host` on windows, but as stated by
  [reference](https://docs.docker.com/network/host/), quote:
  > The host networking driver only works on Linux hosts, and is not supported
  > on Docker Desktop for Mac, Docker Desktop for Windows, or Docker EE for
  > Windows Server. 
  
  So, when you notice something wired, like cannot ping docker exported port
  from windows/wsl2. check if your docker container is launched using
  `--network=host`.
