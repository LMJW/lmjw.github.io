---
date: 2021-12-28
title: 2022-01-05-Simple prost use guid
author: LMJW
tags: [rust, tonic, prost, protobuf, build]
---
# Simple tonic/prost example

## A simple tonic repo setup

Using gRPC usually requires both client & server to have same protobuf definition. Client and server usually are not defined in the same repository.

We can use a separate git repository to store the protobuf definition, and using the git submodule to include the protobuf definition in the corresponding client and server.

Check [sample repo](https://github.com/LMJW/tonic-proto-template) to see how to setup 

## A simple prost example

Sometime you only want to use protobuf to do serializing and deserializing the message, nothing else. In this case, you will need to use prost only.

Here is a simplest example of how to do that.

You may also find the [snazzy](https://github.com/danburkert/snazzy) can be helpful.
