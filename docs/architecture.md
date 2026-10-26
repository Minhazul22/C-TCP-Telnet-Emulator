# Architecture

## Overview

The lab daemon is a single-process TCP server. It monitors the listening socket and active client sockets with `select()`, allowing it to service multiple clients without creating one thread per client.

```mermaid
flowchart TD
    A[Start server] --> B[Create TCP socket]
    B --> C[Bind to port 14055]
    C --> D[Listen for clients]
    D --> E[select: monitor descriptors]
    E --> F{Which descriptor is ready?}
    F -->|Listening socket| G[accept new client]
    G --> H[Initialize client session]
    H --> E
    F -->|Client socket| I[Receive client input]
    I --> J{Session state}
    J -->|Username requested| K[Store username; ask password]
    K --> E
    J -->|Password requested| L[Record login event; show prompt]
    L --> E
    J -->|Logged in| M[Log command]
    M --> N{Command is exit?}
    N -->|Yes| O[Send Bye and close client]
    N -->|No| P[Send unknown command]
    O --> E
    P --> E
```

## Connection lifecycle

1. Create an IPv4 TCP socket.
2. Bind it to all local interfaces on the configured lab port.
3. Start listening for incoming connections.
4. Use `select()` to monitor the listener and connected client sockets.
5. Accept each new connection and initialize per-client state.
6. Read client input and progress through username, password, and logged-in states.
7. For logged-in sessions, log the received command; close the connection for `exit`, otherwise reply `unknown command`.

## Important implementation notes

- The lab version uses a fixed-size client table (64 entries) indexed by socket descriptor. This should be revisited before reuse because file descriptors are not inherently guaranteed to fit within an application-defined client array.
- Robust production code should check all system-call return values and handle partial sends, input framing, descriptor limits, and abnormal disconnects.
- Passwords must never be written to logs in a secure implementation.
