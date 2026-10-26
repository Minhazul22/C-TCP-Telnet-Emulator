# C TCP Telnet Emulator Daemon

A small educational TCP server written in C for Linux. The lab exercise implements a Telnet-style command interface, accepts multiple simultaneous clients with `select()`, handles a simple username/password exchange, and logs session activity.

> **Educational lab project:** This is a simplified emulator, not a real Telnet implementation and not suitable for production use.

## Features

- TCP server using POSIX sockets
- Configurable-at-source listening port (the lab version uses port `14055`)
- Multiple client connections managed with `select()`
- Simple username/password prompt and per-client session state
- Command prompt with an `exit` command; other commands return `unknown command`
- Timestamped session logging

## Architecture

The server creates a TCP listening socket, binds to its port, starts listening, and uses `select()` to monitor the listening socket and connected clients. When a new client connects, it is added to the monitored descriptor set and assigned a session state. Received input advances that client's login state or is handled as a command.

See [architecture notes](docs/architecture.md) and the [server flow diagram](diagrams/server-flow.mmd).

## Build and run

The original lab source is included as `src/nsad.c` only if it is permitted to redistribute. If you have permission to publish your own source, compile it on a Linux system with GCC:

```sh
gcc -Wall -Wextra -o nsad src/nsad.c
./nsad
```

In the lab version, the listening port is defined in the source as `14055`. Connect from a client on a reachable network using:

```sh
nc <server-address> 14055
```

Use only on a system and network where you have permission to run a listening service. Do not expose this lab daemon to the public internet.

## Tested lab behavior

According to the lab notes, testing included a client login, command submissions such as `ls` and `whoami`, the `exit` command, and two simultaneous client sessions. These are reported lab results, not an independent test run of this repository.

See [testing notes](docs/testing.md).

## Security limitations

- The original implementation logs passwords in plaintext. Do not publish real logs or real credentials.
- The login exchange is not encrypted and does not provide secure authentication.
- This is an educational emulator; arbitrary commands are not executed.
- The source should be reviewed for robustness before any use beyond a controlled lab.

## Project context

This repository documents a university networking lab focused on TCP sockets, Linux process/network tools, and concurrent client handling. Lab handout materials or source code should only be committed when redistribution is permitted.

## Skills demonstrated

C, POSIX sockets, TCP/IP, Linux, GCC, `select()`, client/server design, session state management, command parsing, and logging.
