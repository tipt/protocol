# GET_NODE_KEY CNNodeRequest
## Request
`body` must be:
```
map {
    node  str8
          The fully-qualified domain name of the node whose OpenPGP public key
          is being requested.
          Max size: 500 bytes
          Encoding: utf8
}
```

## Response
`body` must be:
```
map {
    key  bin16
         The requested node's OpenPGP public key, un-armored.
         Max size: 2^13 bytes
}
```

`key` is obtained by sending a GET HTTP request to the request `node` parameter as domain with the path `/api/public_key`.
