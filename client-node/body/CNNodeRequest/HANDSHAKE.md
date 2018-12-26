# HANDSHAKE CNNodeRequest
## Request
`body` must be:
```
map {
    ver   str8
          The semantic version number of this specification as used by the
          client.
}
```

## Response
`body` must be:
```
map {
    ver   str8
          The semantic version number of this specification as used by the
          node.

    comp  bool
          Whether the node is compatible with the client version.
}
```

`comp` is determined by checking whether the major version of the request `ver` is lower than or equal to the major version of the response `ver`.

If `comp` is false, the node should disconnect from the client and must not allow any subsequent requests.
