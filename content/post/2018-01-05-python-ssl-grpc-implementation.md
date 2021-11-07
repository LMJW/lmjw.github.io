---
date: 2018-01-05
title: A python example of realizing secure grpc communication
author: LMJW
tags: [grpc, docker, python]
---

# A python example of realizing secure grpc communication

### Useful links and references:

Here are some links that I found it can be helpful when I was trying to work out how to setup the python ssl communication.

[Secure gRPC with TLS/SSL](https://bbengfort.github.io/programmer/2017/03/03/secure-grpc.html), This is golang implementation

[certstrap](https://github.com/square/certstrap#sign-certificate-request-of-host-and-generate-the-certificate), a convienient tool to generate openssl keys and certificate

[gRPC authentication](https://grpc.io/docs/guides/auth.html), the official guide of grpc

[grpcio, python package](https://grpc.io/grpc/python/grpc.html?highlight=credentials#grpc.ssl_server_credentials), the source code and official document of grpc python

[grpc golang ssl example](https://github.com/grpc/grpc-go/blob/master/credentials/credentials.go), another golang example of ssl communication

[What is the difference between .pem , .csr , .key and .crt?](https://crypto.stackexchange.com/questions/43697/what-is-the-difference-between-pem-csr-key-and-crt), a stackexchange question which explains the concepts and differences of different types of files

[What is a Pem file and how does it differ from other OpenSSL Generated Key File Formats?](https://serverfault.com/questions/9708/what-is-a-pem-file-and-how-does-it-differ-from-other-openssl-generated-key-file), a stackexchange question which explains the concepts and differences of different types of files

[Cannot connect to SSL server using IP address](https://github.com/grpc/grpc/issues/2691#issuecomment-154841833), this is a useful thread that helped me to solve the server certificate problem

[TLS Golang Server not working with Node-js and Python Client](https://github.com/grpc/grpc/issues/10062), this thread makes me to understand to set the server certificate to have the name that is the same as the hostname.



Go to my [GITHUB](https://github.com/LMJW/grpc_ssl) repository to see the source code implementation.