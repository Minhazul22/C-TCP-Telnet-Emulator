# Lab Testing Notes

The following behaviors are reported in the lab summary. They are recorded as historical lab observations, not as tests rerun against this repository.

| Scenario | Reported result |
|---|---|
| Connect with `nc` to the server on port `14055` | Username prompt appeared |
| Enter username and password | Login-success message and command prompt appeared |
| Submit `ls` and `whoami` | Server responded `unknown command` |
| Submit `exit` | Server responded `Bye!` and closed the session |
| Connect two clients concurrently | Both sessions remained independently usable |
| Inspect daemon log | Session events and commands were recorded with timestamps |

## Suggested repeatable checks

Run only in a controlled lab network where you are authorized to operate the service.

1. Compile with warnings enabled: `gcc -Wall -Wextra -o nsad src/nsad.c`.
2. Start the server and connect using `nc <server-address> 14055`.
3. Verify the username/password prompt sequence.
4. Check the response to an ordinary command and to `exit`.
5. Connect two clients simultaneously and verify that input from one session does not appear in the other.
6. Inspect logs only after removing usernames, passwords, addresses, and other identifying or sensitive information.

## Known security concern

The lab source logs the entered password in plaintext. Treat the original log as sensitive and do not commit it. A future revision should remove password storage and ensure the logging function cannot receive password values.
