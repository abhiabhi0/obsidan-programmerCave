```
Client                          Server
  |                               |
  |------- Login Request -------->|
  |                               |
  |<---- JWT (if authenticated)---|
  |                               |
  |--- Request with JWT Token --->|
  |                               |
  |--- Verify JWT --------------->|
  |                               |
  |<----- Protected Resource -----|
  |                               |

```

