---
date: 2021-11-20
title: Painless key mapping on windows
author: LMJW
tags: [tool, key-mapping]
---

Recently, I start to using windows for my personal project. One thing I hate about is the keyboard mappings.

I want to basically do the following mappings

> 1 Caps lock key -> Left Ctrl key
> 2 Left Alt key -> Left Ctrl key
> 3 Left Windows key -> Left Alt key

I was using the . The first two key mapping works fine, however, the third mapping has the problem. For example, "Win+W" should suppose to map to "Alt+W" which corresponding to the "Copy" in emacs, however, it opens the "Pen" app in the Windows.

I did some research, and found another tool, which called the [AutoHotKey](https://www.autohotkey.com/). Although this tool is quite nice to do some specific customizations, it is not good for my perticular use cases. By looking through the guide on https://www.autohotkey.com/docs/misc/Remap.htm#registry, it points out to a different application, which is called [KeyTweak](https://www.bleepingcomputer.com/download/keytweak/).

By using the KeyTweak, I was able to map the "Win" key to "Alt" key without having any issues.

---

### Useful infos
- https://superuser.com/questions/1147517/how-to-disable-only-some-windows-10-global-shortuts-to-use-them-in-third-party-a
- https://www.autohotkey.com/docs/misc/Remap.htm#registry
