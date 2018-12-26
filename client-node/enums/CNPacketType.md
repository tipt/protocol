# CNPacketType
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
