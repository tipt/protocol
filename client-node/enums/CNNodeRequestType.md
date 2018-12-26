# CNNodeRequestType
Used in the root object to indicate the type of request when the request is sent **to** the node.

All requests marked as *“no sign in”* must only be performed by clients that have not signed in. All requests marked as *“no sign in required”* may be performed by all clients whether signed in or not. All other requests must only be performed by users who have already signed in.

The `HANDSHAKE` request must be made before any other request. If this request is not made within 3 seconds after the WebSocket has fully connected, the node should disconnect the client with an HTTP 400 status.

positive fixint (max 127)

```
0x00    HANDSHAKE       The client wants to ensure it's compatible with the
                        client-node protocol version the server is running.

0x01    REGISTER        (no sign in) The user wants to register on the node.

0x02    SIGN_IN         (no sign in) The client wants to sign in.

0x03	GET_INFO		(no sign in required) Obtains information on the node.

0x04-0x1f               Reserved for future requests made by clients that
                        have not signed in

0x20    FORWARD         The client requests that a message be forwarded to
                        another node.
```
