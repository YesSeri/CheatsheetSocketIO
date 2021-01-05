# Intro

This is a cheat sheet for SocketIO, because I found the documentation lacking.
## Foreword

This is for Node.js and React. Most of this is about Node.js, so even if you don't use React, you should be able to use a lot.



# Main

## Boilerplate

Make a new repo for your with either

```bash
yarn
```

and install the relevant packages with the following command:

```bash
yarn add express socket.io
```

Use this boilerplate to get up and running quickly

```javascript
const app = require('express')();
const httpServer = require('http').Server(app);
const io = require('socket.io')(httpServer);

// In here goes all the Socket IO logic and server logic

const PORT = process.env.PORT || 5000;
http.listen(PORT, () => {
    console.log(`Listening at port ${PORT}`);
});
```

For React boilerplate see the client connection in the next section. You will want to install Socket.IO client. Use the following command:

```bash
yarn add socket.io-client
```

Now we will learn how to use Socket.IO in detail.

## Connection

This establishes a connection between the computer and the server. On connection the first callback function executes. On disconnect, the dc callback function executes.

### Server

```javascript
io.on('connection', (socket) => {
    console.log('User Connected');
    socket.on('disconnect', () => {
        console.log('User Disconnected');
    });
});
```

### Client

This establishes a socket between server and client and listens for an event call 'serverEvent'.

```javascript
import React, { useState, useEffect } from 'react';
import socketIOClient from 'socket.io-client';

const ENDPOINT = 'http://127.0.0.1:5000';

const socket = socketIOClient(ENDPOINT);

const Test = () => {
  const [data, setdata] = ("");
  useEffect(() => {
        socket.on('serverEvent', (payload) => {
            setData(data);
        });                                                // The return function in useEffect is executed
        return socket.disconnect; // when user leaves site or page
    }, []);
}

export default Test;
```

## Emit

### Basics.

The first value is the channel, and the second one is the message being emitted.

### Server & Client

This emits hey on the test channel. This is between one computer \(the one on the socket\) and the server.

```javascript
socket.emit('test', 'hey');
```

### Server

This emits to all sockets, except the connected socket.

```javascript
socket.broadcast.emit('test', 'hey');
```

Use example:

```javascript
// One to one communication
socket.emit('newclientconnect', 'Hi there new user');
// To everybody else
socket.broadcast.emit('newclientconnect', 'A new user has joined');
```

This emits to all connected sockets on the channel 'test'. This only works serverside.

```javascript
io.on('connection', (socket) => {
  io.sockets.emit('test','hey');
}
```

## Recieve

Recieve like this. Test is the event being emitted, and payload the info\('hey'\). You can think of the event being emitted as the the channel on which you recieve things. 

```javascript
socket.on('test', (payload) => {
    console.log(payload);
});
```

## Namespace

Default namespace is '/'.

### Server

A space called '/my-namespace' is created. Use nsp.emit to use it.

```javascript
const nsp = io.of('/my-namespace');
nsp.on('connection', function (socket) {
    nsp.emit('hi', 'Hello everyone!');
});
```

### Client

Joins a namespace. Used to clearly seperate people, and divide the load on server. In discord, it is like channel. The channel is the subdivided into rooms. Combined with the server code above, this will lead to that when someone joins they will recieve a 'hi' event containing 'Hello everyone'.

```javascript
const socket = io('/my-namespace');
socket.on('test', function (data) {
    console.log(data);
});
```

## Room

### Server

This makes socket join a room called 'testRoom' and emits event 'serverEvent' with the message 'hey' to all in the room. The logic for which room is joined only happens on the serverSide. There is no `join()` function for clientside.

```javascript
io.on('connection', (socket) => {
  socket.join('testRoom');
  io.sockets.in(room).emit('serverEvent', 'hey');
});
```

If you want to choose what room to join from the client side, you can use this snippet.

#### Server

```javascript
io.on('connection', (socket) => {
  socket.on('join', (room) => {
    socket.join(room);
  });
});
```

#### Client

```javascript
socket.emit('join', 'roomName');
```

# Error Handling

## Error handling

The client API has the following functions.

* **connect** − When the client successfully connects.
* **connecting** − When the client is in the process of connecting.
* **disconnect** − When the client is disconnected.
* **connect\_failed** − When the connection to the server fails.
* **error** − An error event is sent from the server.
* **message** − When the server sends a message using the **send** function.
* **reconnect** − When reconnecting to the server is successful.
* **reconnecting** − When the client is in the process of connecting.
* **reconnect\_failed** − When the reconnect attempt fails.

One example would be:

```javascript
socket.on('connect_failed', () => {
  console.log('Connection failed'
})
```

