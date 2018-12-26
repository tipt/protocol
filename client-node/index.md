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

## Enums
### CNPacketType
Used in the root object to indicate the context of the root blob.

positive fixint (max 127)

```
0x00    REQUEST
        Indicates a request that demands a response.
        When set, `/reqt` must be a `CNClientRequestType` or `CNNodeRequestType`.

0x01    RESPONSE
        Indicates a response to a REQUEST.
        When set, `/reqt` must either not be present or be nil.

0x02    EVENT
        Sent when one party has information for the other party which is not
        sent as part of a response. No response is needed.
        When set, `/reqt` must be a `CNClientEventType` or `CNNodeRequestType`.
```

### CNNodeRequestType
Used in the root object to indicate the type of request when the request is sent **to** the node.

All requests marked as *“no sign in”* must only be performed by users that have not signed in. All other requests must only be performed by users who have already signed in.

The `HANDSHAKE` request must be made before any other request.

positive fixint (max 127)

```
0x00    HANDSHAKE       The client wants to ensure it's compatible with the
                        client-node protocol version the server is running.

0x01    REGISTER        (no sign in) The user wants to register on the node.

0x02    SIGN_IN         (no sign in) The client wants to sign in.

0x03-0x1f               Reserved for future requests made by clients that
                        have not signed in

0x20    FORWARD         The client requests that a message be forwarded to
                        another node.
```
