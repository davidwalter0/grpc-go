# Description
The route guide server and client demonstrate how to use grpc go libraries to
perform unary, client streaming, server streaming and full duplex RPCs.

Please refer to [gRPC Basics: Go] (http://www.grpc.io/docs/tutorials/basic/go.html) for more information.

See the definition of the route guide service in proto/route_guide.proto.

# Run the sample code
To compile and run the server, assuming you are in the root of the route_guide
folder, i.e., .../examples/route_guide/, simply:

```sh
$ go run server/server.go
```

Likewise, to run the client:

```sh
$ go run client/client.go
```

# Optional command line flags
The server and client both take optional command line flags. For example, the
client and server run without TLS by default. To enable TLS:

As a trivial cert generation you 

```sh
$ go run server/server.go -tls=true
```

and

```sh
$ go run client/client.go -tls=true
```
---

Modifications to test certificate generation with grpc example using
certstrap which writes to dir out.

- https://github.com/square/certstrap

```
$ certstrap init --stdout --common-name "RootCA" ; certstrap sign --CA "RootCA"  --years 10 RootCA
$ certstrap request-cert --domain example.com; certstrap sign --CA "RootCA" example.com
$ mv out certs
```

Then either

```
$ go run server/server.go --tls \
         --cert-file certs/example.com.crt --root-ca certs/RootCA.crt --key-file certs/example.com.key 

$ go run client/client.go --tls \
         -server-addr example.com:10000 --server-host-override example.com \
         --ca-file certs/RootCA.crt  --cert-file certs/example.com.crt --key-file certs/example.com.key 
```
or
```
$ go run server/server.go --tls

$ go run client/client.go --tls

```
