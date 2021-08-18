# Solo Wallet Transaction Delivery WebSockets API

## Keeping Connection Alive

For keeping WS connection alive client have to implement PING/PONG mechanism.

Each 5 seconds client have to send a 'ping' message, and the server will respond with 'pong' message.

If client doesn't send 'ping' message within 15 seconds after connecting or last 'ping' it will be disconnected by the server.

## Issuer

### Subscribe to Transaction updates
`/ws/v1/issuer/transactions/:identifier`

Sends transaction metadata properties whose values have changed in the messages.

When transaction expires [expire message](ws.md#transaction-expired-message) will be sent.
Client should close connection otherwise it will be automatically closed in 5 seconds.

#### Examples

##### Connecting

```bash
 wscat -c 'wss://api.test.sologenic.org/ws/v1/issuer/transactions/5a35e17a-c5e1-4f88-9e39-968eaa18cd43'
```

##### Transaction opened message
```json5
{
  "meta": {
    "identifier": "5a35e17a-c5e1-4f88-9e39-968eaa18cd43",
    "opened": true
  }
}
```

##### Transaction cancelled message
```json5
{
  "meta": {
    "identifier": "5a35e17a-c5e1-4f88-9e39-968eaa18cd43",
    "cancelled": true
  }
}
```

##### Transaction signed message
```json5
{
  "meta": {
    "identifier": "5a35e17a-c5e1-4f88-9e39-968eaa18cd43",
    "signed": true,
    "push_token": "eyJhbGciOiJFUzUxMiIsInR5cCI6IkpXVCJ9.eyJhdWQiOiJzZ190eF9kZWxpdmVyeSIsImV4cCI6MTYzMTM2Mjc1NiwianRpIjoiZGE2YzMzYjItZGE4ZS00NmJkLThiOWYtYmU4YWRhNDM4Y2NmIiwiaWF0IjoxNjI4NzcwNzU2LCJhZGRyZXNzIjoickVhV3BVR0NURThuRG1MUjJERVF6a1IxRXJmcmYxalJDdSJ9.AR0TGiliEWg3bUljStLphJZS2j-vp8zJLO7aWEWTXyCYQt3MYZSDx6zp70GCf8FXR0nAPrhRHCNz-7wWjNlYFfgjAfLhUmxteLSXTsgjfG6JUMx-wq-w1_miSkmNSCU41xZqsWfNys8xIDABNBbhf-npJt0CkDK-rpIzFwh3HmVY961_"
  }
}
```

##### Transaction expired message
```json5
{
  "meta": {
    "identifier": "5a35e17a-c5e1-4f88-9e39-968eaa18cd43",
    "expired": true
  }
}
```

#### Errors

Code **3100** invalid transaction subscription. Reasons:
- `transaction.not_found`
- `transaction.identifier.invalid`
- `transaction.resolved`
- `transaction.expired`
