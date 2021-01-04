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

