# C TCP Telnet Emulator Daemon

A documentation-focused portfolio entry for a university networking lab. The lab exercise involved building a Telnet-style TCP server in C that handled multiple clients with `select()`, prompted for a username and password, responded to commands, and logged session activity.

> **Educational lab project:** This was a simplified emulator, not a real Telnet implementation and not suitable for production use.

## What the lab covered

- TCP server concepts and POSIX sockets
- Listening on lab port `14055`
- Multiple client connections managed with `select()`
- Per-client username/password prompt and session state
- A command prompt where `exit` closes the session and other commands return `unknown command`
- Timestamped session logging
- Testing simultaneous client sessions with `nc`

## Architecture

The server creates a TCP listening socket, binds it to its port, starts listening, and uses `select()` to monitor the listening socket and connected client sockets. New connections are accepted and assigned session state. Received input advances the login flow or is handled as a command.

See [architecture notes](docs/architecture.md) and the [server flow diagram source](diagrams/server-flow.mmd).

## Source code availability

The source code is **not included in this public repository** because permission to redistribute the previous-student starter example was not granted. The cleaned derivative is not published either. This repository therefore presents the project through original explanatory documentation rather than redistributing code that may be restricted.

The original source should remain in private course storage. Any independently written implementation should be clearly separated from restricted starter material and reviewed before public release.

## Reported lab testing

According to the lab summary, testing included a client login, command submissions such as `ls` and `whoami`, the `exit` command, and two simultaneous client sessions. These are reported lab results, not an independent test run of code in this repository.

See [testing notes](docs/testing.md).

## Security lessons

- The original lab implementation wrote entered passwords to its log in plaintext. Do not publish the original log or real credentials.
- The login exchange was not encrypted and did not provide secure authentication.
- The server was an educational emulator; arbitrary commands were not executed.
- Fixed-size client tracking and robust socket error handling need careful review before any implementation is reused.

## Project context

This repository documents a university networking lab focused on TCP sockets, Linux network tools, client/server design, and concurrent connection handling. Supporting course materials and code should only be redistributed with permission.

## Skills demonstrated

C and POSIX socket concepts, TCP/IP, Linux, `select()`, client/server design, session state management, command handling, logging, and network testing.
