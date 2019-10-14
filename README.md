# Socket.IO C++ Client
This is a fork of C++ implementation of socket.io-client-cpp from [socket.io-clientpp](https://github.com/socketio/socket.io-client-cpp)

## Features

- 100% written in modern C++11
- Compatible with socket.io 1.0+ protocol
- Binary support
- Automatic JSON encoding
- Multiplex support
- Similar API to the Socket.IO JS client
- Cross platform

## Installation alternatives

* [With CMAKE](./INSTALL.md#with-cmake)
* [Without CMAKE](./INSTALL.md#without-cmake)


## Quickstart

** [Full overview of API can be seen here](./API.md) **


The APIs are similar to the JS client.

#### Connect to a server
```C++
sio::client h;
h.connect("http://127.0.0.1:3000");
```

#### Emit an event

```C++
// emit event name only:
h.socket()->emit("login");

// emit text
h.socket()->emit("add user", username);

// emit binary
char buf[100];
h.socket()->emit("add user", std::make_shared<std::string>(buf,100));

// emit message object with lambda ack handler
h.socket()->emit("add user", string_message::create(username), [&](message::list const& msg) {
});

// emit multiple arguments
message::list li("sports");
li.push(string_message::create("economics"));
socket->emit("categories", li);
```
Items in `message::list` will be expanded in server side event callback function as function arguments.

#### Bind an event

##### Bind with function pointer
```C++
void OnMessage(sio::event &)
{

}
h.socket()->on("new message", &OnMessage);
```

##### Bind with lambda
```C++
h.socket()->on("login", [&](sio::event& ev)
{
    //handle login message
    //post to UI thread if any UI updating.
});
```

##### Bind with member function
```C++
class MessageHandler
{
public:
    void OnMessage(sio::event &);
};
MessageHandler mh;
h.socket()->on("new message",std::bind( &MessageHandler::OnMessage,&mh,std::placeholders::_1));
```

#### Using namespace
```C++
h.socket("/chat")->emit("add user", username);
```
** [Full overview of API can be seen here](./API.md) **

## License

MIT
