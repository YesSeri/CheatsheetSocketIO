---
description: 'This is a cheat sheet for SocketIO, because I found the documentation lacking.'
---

# Intro

## Foreword

This is for Node.js and React. Most of this is about Node.js, so even if you don't use React, you should be able to use a lot.

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

