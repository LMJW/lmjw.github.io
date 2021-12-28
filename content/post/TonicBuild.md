---
date: 2021-12-28
title: A nice way to setup tonic repo
author: LMJW
tags: [rust, tonic, build]
---

## A nice way to setup tonic repo

Using gRPC usually requires both client & server to have same protobuf definition. Client and server usually are not defined in the same repository.

We can use a separate git repository to store the protobuf definition, and using the git submodule to include the protobuf definition in the corresponding client and server.

Check [sample repo](https://github.com/LMJW/tonic-proto-template) to see how to setup 