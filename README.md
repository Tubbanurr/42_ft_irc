# 42_ft_irc
42 Ecole Common Core Projects / ft_irc


# 🖧 ft\_irc - Internet Relay Chat Server

## 📌 Project Overview

**ft\_irc** is a simple IRC server built in **C++98**, following the **IRC (Internet Relay Chat) protocol**.\
It allows multiple clients to communicate via TCP/IP using an IRC client of their choice.

---

## 🚀 Features

- **Multi-client support** using **poll()** (or equivalent)
- **Authentication system** (`PASS`, `NICK`, `USER` commands)
- **Real-time messaging** (public channels and private messages)
- **User roles** (Operators and regular users)
- **Implemented IRC commands:**
  - `JOIN` - Join a channel
  - `PART` - Leave a channel
  - `PRIVMSG` - Send private messages
  - `KICK` - Remove a user from a channel
  - `INVITE` - Invite a user to a channel
  - `TOPIC` - Change or view the channel topic
  - `MODE` - Manage channel modes (`+i`, `+t`, `+k`, `+o`, `+l`)
  - `QUIT` - Disconnect from the server
- **Message broadcasting** - Messages sent to a channel are received by all members.
- **Error handling** - Provides warnings for invalid commands and incorrect inputs.
- **Clean and modular code** following **OOP (Object-Oriented Programming)** principles.

---

## ⚙️ Installation & Compilation

### Prerequisites

- **C++98 Compiler**
- **An IRC client** (e.g., `irssi`, `WeeChat`, `HexChat`, or `netcat` for testing)

### Compilation

To build the project, run:

```sh
make
```

This will generate an executable called `ircserv`.

---

## 🔧 Usage

To start the server, run:

```sh
./ircserv <port> <password>
```

- `<port>`: The port number on which the IRC server will listen for incoming connections.
- `<password>`: The connection password required by clients.

---

## 🔗 Connecting to the IRC Server

### 🔹 Terminal Connection (Testing with `HexChat`)

For a simple connection test, run:

```sh
nc -C 127.0.0.1 6667
```

Then, authenticate with:

```irc
PASS mypassword
NICK mynickname
USER myusername 0 * :My Real Name
```

To join a channel:

```irc
JOIN #mychannel
```

---

## 📜 Supported IRC Commands & Examples

| Command   | Description                   | Example                         |
| --------- | ----------------------------- | ------------------------------- |
| `PASS`    | Authenticate with a password  | `PASS mypassword`               |
| `NICK`    | Set a nickname                | `NICK Alice`                    |
| `USER`    | Set username and real name    | `USER Alice 0 * :Alice Doe`     |
| `JOIN`    | Join a channel                | `JOIN #channelname`             |
| `PART`    | Leave a channel               | `PART #channelname`             |
| `PRIVMSG` | Send a private message        | `PRIVMSG Bob :Hello!`           |
| `KICK`    | Remove a user from a channel  | `KICK #channelname Bob :Reason` |
| `INVITE`  | Invite a user to a channel    | `INVITE Bob #channelname`       |
| `TOPIC`   | View or set the channel topic | `TOPIC #channelname :New topic` |
| `MODE`    | Manage channel settings       | `MODE #channelname +i`          |
| `QUIT`    | Disconnect from the server    | `QUIT`                          |

---

## 🖧 **Socket Programming & Server Logic**

This IRC server uses **socket programming** to communicate with clients. A **socket** is an endpoint for data exchange over a network.

### 🏗️ **Socket Workflow**

1. **Create a socket** - `socket()` function initializes a socket.
2. **Bind to a port** - `bind()` associates the socket with a specific **port**.
3. **Listen for connections** - `listen()` puts the socket in listening mode.
4. **Accept client connections** - `accept()` handles incoming client requests.
5. **Send/Receive data** - `send()` and `recv()` process messages between server and clients.
6. **Close connections** - `close()` terminates client connections.

### **Main Server Loop**

The server continuously monitors connected clients using **poll()**:

```cpp
int retval = poll(&_pollFds[0], _pollFds.size(), -1);
if (retval == -1) {
    std::cerr << "poll Error: " << strerror(errno) << std::endl;
}
```

This enables **non-blocking socket communication**, ensuring efficient client management.

---

## 📂 Project Structure

```
├── src/
│   ├── Server.cpp       # Server logic
│   ├── Client.cpp       # Client management
│   ├── Channel.cpp      # Channel operations
│   ├── Cmds.cpp         # Core commands
│   ├── Cmds1.cpp        # Additional commands
│   ├── Cmds2.cpp        # Mode handling
│   ├── Tools.cpp        # Utility functions
│   ├── main.cpp         # Entry point
├── includes/
│   ├── ft_irc.hpp       # Main header file
│   ├── rpls_and_errs.hpp # Reply and error messages
├── Makefile             # Build script
├── README.md            # Documentation
```

---

## 🏆 **Bonus Features**
**Note:** THERE IS NO BONUS PART IN THIS PROJECT!

- 📂 **File transfer support** (Client-to-client file sharing)
- 🤖 **Bot integration** (Automated message response bot)

**Note:** Bonus features will only be evaluated if all mandatory requirements are met.

---

## 🤝 Contributing

If you'd like to contribute, feel free to fork the repository and submit a pull request. Keep the code **clean and well-documented**. 💡

---

## 🎉 Special Thanks

This project was developed with my teammates **ydunay** and **zeatilga**.\
Thanks to my team for their support! 🙌

---

🚀 **Happy coding!** 💻🎯

