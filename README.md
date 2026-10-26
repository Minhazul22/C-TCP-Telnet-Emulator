# C TCP Telnet Emulator Daemon

A small educational TCP server written in C for Linux. The lab exercise implements a Telnet-style command interface, accepts multiple simultaneous clients with `select()`, handles a simple username/password exchange, and logs session activity.

> **Educational lab project:** This is a simplified emulator, not a real Telnet implementation and not suitable for production use.

## Features

- TCP server using POSIX sockets
- Lab listening port: `14055`
- Multiple client connections managed with `select()`
- Simple username/password prompt and per-client session state
- Command prompt with an `exit` command; other commands return `unknown command`
- Timestamped session logging

## Architecture

The server creates a TCP listening socket, binds it to its port, starts listening, and uses `select()` to monitor the listening socket and connected clients. New connections are added to the monitored descriptor set. Received input advances that client's login state or is handled as a command.

See [architecture notes](docs/architecture.md) and the [server flow diagram source](diagrams/server-flow.mmd).

## Source and build status

This repository currently contains project documentation and diagram source; the original C source is not included yet. The lab implementation was developed from an existing example/template, so check the course's redistribution rules before publishing the source.

After an approved and reviewed source file is added at `src/nsad.c`, the intended build command on Linux is:

```sh
gcc -Wall -Wextra -o nsad src/nsad.c
./nsad
```

In the lab version, the listening port is defined in the source as `14055`. A client can connect from an authorized machine using:

```sh
nc <server-address> 14055
```

Use only on a system and network where you have permission to run a listening service. Do not expose this lab daemon to the public internet.

## Reported lab testing

According to the lab summary, testing included a client login, command submissions such as `ls` and `whoami`, the `exit` command, and two simultaneous client sessions. These are reported lab results, not an independent test run of this repository.

See [testing notes](docs/testing.md).

## Security limitations

- The uploaded lab implementation writes entered passwords to its log in plaintext. Do not publish the original log or any real credentials.
- The login exchange is not encrypted and does not provide secure authentication.
- This is an educational emulator; arbitrary commands are not executed.
- The original implementation should be reviewed for robustness before any use beyond a controlled lab.
- The client table is fixed-size and indexed by socket descriptor in the supplied implementation; this needs to be made safe before reusing the code.

## Project context

This repository documents a university networking lab focused on TCP sockets, Linux process/network tools, and concurrent client handling. Lab handout materials or code should only be committed when redistribution is permitted.

## Skills demonstrated

C, POSIX sockets, TCP/IP, Linux, GCC, `select()`, client/server design, session state management, command parsing, and logging.
