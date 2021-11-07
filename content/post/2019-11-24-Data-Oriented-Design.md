---
date: 2019-11-24
title: Data Oriented Design
author: LMJW
tags: [cpp]
---

# Data Oriented Design

Check out the video. This explains the AOS and SOA very well.
[data orientied design](https://www.youtube.com/watch?v=ScvpoiTbMKc)

---

The following code is a common pattern in Object oriented programming.

```Cpp
struct Entity{
  v3 postion,
  v3 velocity,
  int flag,

  virtual void update()
}

struct Player: public Entity{
  float life,
  float mana,

  void update() override;
}

struct Monster: public Entity{
  float life,

  void update() override;
}

sturct Door: public Entity{
  bool current_status,
  float open_target,

  void update();
}
```

There are few problems with this approach:

1. Memory allocation. In this objected orientied approach, because we inherited Entity objects and added some fields in child object, the size of the new Object can have different sizes. So, we may ends up as many different size objects which will imply random heap allocation in memory. (just like objects are putting every where in memory without organize) So when we want to access these objects, the speed of accessing all these object can be a bit slow.

2. L1 Cache miss. Let's say we want to update the postion of a player, we use the equation `'p = p + v*dt`. To update the object state, we will need to know two fields stored in the obejct, the `position` and the `velocity` of the object. But, to access these fields, we will need to get all the structure in the L1 cache. So in the end, we fetched all the struct into L1 cache, but we only used two fields. This is something called caches misses.

3. The object oriented approach is also messy in terms of state management, and this can be expensive. Say a player hit a monster, the monster has its own state like how much life it has. But because of object oriented design, the state is kept private to monster, so eventually monster will need to have a method like `get_damage`. Eventually, we will need to load `player` object and then call the `get_damage` method of monster object. But what all we do is probably manipulate the `life` field, but we will need to fully load the `player` struct and `monster` struct, which may contain many other fields and method.

---

So, what is data oriented design and how can it solve this problem?

The difference of object oriented design and data oriented design is a difference between `Array Of Struts`(AOS) and `Strut of Arrays`(SOA). Lets see how AOS and SOA are stored in memory

```Cpp
struct ood{
  type_A a,
  type_B b,
  type_C c
}

struct dod{
  type_A a[1000],
  type_B b[1000],
  type_C c[1000]
}

// memory
// OOD (Object orentied design)
// ...|a|b|c|...|a|b|c|...|a|b|c|...
//
// DOD (data orentied design)
// ...|a|a|a|...|b|b|b|...|c|c|c|...
//
// say we need to update a&b field for all structs. The first OOD case, we will
// need to go through all the structs, fetch the whole struct into L1 cache, then
// update field a,b, then iterate all the structs. Although we read C field in the
// L1 Cache, we did not use it at all. We have about 33% cach missing rate. This
// number is more significant if the object is bigger.
//
// But for DOD case, we will just need to pull all field of a and b, and just updates
// these fields. We have almost 100% cache usagage, which will be much faster.
```

For the latter case, because in the memory, the same data is more closely aligned with same type, therefore it is more easy for cpu to do hardware optimization(hardware prefetching). In addition to that, this kind of structure makes SIMD (single instruction mulitple data) very easy.

What is SIMD?

SIMD is kind like hardware circle for some instruction which runs really fast and in paralle. SIMD allows same instructs for different data to execute in paralle to speed up the computation. SIMD is supported by some modern CPUs. With data orentied design, it can better utilize the SIMD to do the calculation as all our data is aligned nicely in memory.
