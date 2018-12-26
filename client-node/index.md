# The Tipt Client-Node Protocol v1.0.0
Tipt clients communicate with Tipt nodes using the following protocol specification.

## Introduction
Each client must connect with a node using a secure WebSocket on port 443. Invalid SSL certificates must not be accepted.

The node must never reject connections on grounds other than overloading, maintenance or internal errors. Any rejected connection should receive an HTTP 5xx status code as response.

This protocol is bi-directional, meaning that both the client and the node may request one another. Each request must receive a response.

Both the client and node must respond to WebSocket `PING`s with a respective `PONG`. Clients should send a `PING` every five minutes to prevent NAT routers from dropping inactive TCP connections.

## Request format
Every single request must be formatted according to the msgpack specification. As msgpack allows integers of arbitrary size, the Tipt protocols apply their own limits in order to aid implementations in strictly typed languages. For this reason even implementations in loosely typed langnuages must adhere to these restrictions.

Naturally, if using a smaller data type in the encoded msgpack blob than the one specified in the specification is possible this should be done. E.g. if the specification says to use int16, an int16 should be used by the program, but when encoding it may be turned into an int8 or a positive fixint if the value permits doing so.

Every request must adhere to the following format:
```
{
    ver   str8
          The semantic version number of this specification.
    id    uint8
          The id of the request to be included in the response. This value must
          only be reused when a response has been issued for the respective
          request.
          This value is only unique per directional request, i.e. the same
          value may be used in a client-node and node-client request
          simultaneously without any side-effects.
    pakt  CNPacketType
    reqt  ?
          See CNPacketType for type.
}
```
