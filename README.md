# FT_IRC

## Description

`ft_irc` is a lightweight Internet Relay Chat (IRC) server the goal is to implement an IRC server that can accept multiple clients, register users, create and manage channels, messages and permissions.

The server uses one non-blocking event loop driven by `poll()`. Incoming TCP data is buffered and parsed into IRC lines, while outgoing messages are queued so that partial `send()` operations are handled safely. The repository also includes an optional IRC bot that joins a channel and responds to `!help`, `!time`, and `!ping`.

### Supported functionality

- Password-protected client registration with `PASS`, `NICK`, and `USER`.
- Multiple simultaneous clients through non-blocking sockets and `poll()`.
- Private and channel messaging with `PRIVMSG`.
- Channel creation and membership with `JOIN` and `PART`.
- Topics with `TOPIC`.
- Channel administration with `KICK`, `INVITE`, and `MODE`.
- Channel modes `+i` (invite-only), `+t` (operator-only topics), `+k` (key), `+l` (user limit), and `+o` (operator).
- Optional `ircbot` bonus executable.


### Compilation

`mandatory`

```sh
make
```

`bonus`

```sh
make bonus
```
`flags`

```sh
-Wall -Wextra -Werror -std=c++98
```

### server side


The server expects a port between `1024` and `65535` and a non-empty password without whitespace:

```sh
./ircserv <port> <password>
```

Example:

```sh
./ircserv 6667 secret
```

### client side

```sh
nc 127.0.0.1 6667
PASS secret
NICK name
USER name 0 * :name nickname
```

After registration, try commands such as:

```text
JOIN #general
PRIVMSG #general :Hello everyone!
TOPIC #general :A test channel
MODE #general +i
PART #general :Leaving
QUIT :Goodbye
```

Press `Ctrl-C` to stop the server cleanly.

### Running bonus bot

Start `ircserv` first, then run:

```sh
./ircbot <host> <port> <password> [nick] [channel]
```

Example:

```sh
./ircbot 127.0.0.1 6667 secret ircbot '#bot'
```

The bot registers, joins the selected channel, and responds to `!help`, `!time`, and `!ping`.
## Resources

- [RFC 1459: Internet Relay Chat Protocol](https://www.rfc-editor.org/rfc/rfc1459) — original IRC protocol specification.
- [RFC 2812: Internet Relay Chat: Client Protocol](https://www.rfc-editor.org/rfc/rfc2812) — client commands, registration, messages, and replies.
- [RFC 2811: Internet Relay Chat: Channel Management](https://www.rfc-editor.org/rfc/rfc2811) — channel rules and modes.
- [`socket(2)` Linux manual page](https://man7.org/linux/man-pages/man2/socket.2.html) — TCP socket creation.
- [`poll(2)` Linux manual page](https://man7.org/linux/man-pages/man2/poll.2.html) — event multiplexing used by the server and bot.
- [Beej's Guide to Network Programming](https://beej.us/guide/bgnet/) — practical reference for POSIX socket programming.

### Use of AI

AI was used only to help understand the IRC protocol, networking, and C++ concepts involved in the project, and to provide guidance on the steps needed to code the project.
