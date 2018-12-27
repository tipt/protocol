# FORWARD CNNodeRequest
## Request
`body` must be:
```
map {
    node  str8
          The fully-qualified domain name of the node to forward the message
          to.
          Max size: 500 bytes
          Encoding: utf8

    wait  bool
          Whether to wait for the node to respond and send the response back
          to the client.

    msg   bin32
          The encrypted CNNodeForwardRequest to send to the node.
          Must be encrypted using the recipient node's OpenPGP public key.
          Max size: 2MB
}
```

## Response
`body` must be `nil` if `wait` is `false` in the request.

Otherwise the node must wait for the recipient node to respond and then forward that as a `bin32` in `body`.

In order to forward the request, the node must send the request `msg` as a payload in a POST request to the domain name specified in the request `node` parameter at the path `/api/forward`.
