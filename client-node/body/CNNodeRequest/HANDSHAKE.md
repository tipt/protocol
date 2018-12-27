# HANDSHAKE CNNodeRequest
## Request
`body` must be:
```
map {
    ver   str8
          The semantic version number of this specification as used by the
          client.
          Max size: 64 bytes
          Encoding: ASCII
}
```

## Response
`body` must be:
```
map {
    ver   str8
          The semantic version number of this specification as used by the
          node.
          Max size: 64 bytes
          Encoding: ASCII

    comp  bool
          Whether the node is compatible with the client version.
}
```

`comp` is determined by checking whether the major version of the request `ver` is lower than or equal to the major version of the response `ver`.

If `comp` is false, the node should disconnect from the client and must not allow any subsequent requests.
