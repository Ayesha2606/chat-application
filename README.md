# chat-application
// index.js
const express = require('express');
const http = require('http');
const socketIo = require('socket.io');

const app = express();
const server = http.createServer(app);
const io = socketIo(server);

app.use(express.static('public'));

io.on('connection', (socket) => {
  console.log('a user connected');

  socket.on('disconnect', () => {
    console.log('user disconnected');
  });

  socket.on('chat message', (msg) => {
    io.emit('chat message', msg);
  });
});

const PORT = process.env.PORT || 3000;
server.listen(PORT, () => {
  console.log(`Server is running on port ${PORT}`);
});

<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Chat Application</title>
  <link rel="stylesheet" href="styles.css">
</head>
<body>
  <div class="chat-container">
    <div id="chat-box" class="chat-box"></div>
    <form id="chat-form" class="chat-form">
      <input id="message-input" type="text" autocomplete="off" placeholder="Type a message..." />
      <button type="submit">Send</button>
    </form>
  </div>
  <script src="/socket.io/socket.io.js"></script>
  <script src="main.js"></script>
</body>
</html>

body {
  display: flex;
  justify-content: center;
  align-items: center;
  height: 100vh;
  margin: 0;
  font-family: Arial, sans-serif;
  background-color: #f0f0f0;
}

.chat-container {
  width: 400px;
  max-width: 100%;
  border: 1px solid #ccc;
  border-radius: 10px;
  overflow: hidden;
  box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
  background-color: #fff;
}

.chat-box {
  height: 300px;
  overflow-y: scroll;
  padding: 10px;
  border-bottom: 1px solid #ccc;
}

.chat-form {
  display: flex;
  padding: 10px;
  background-color: #eee;
}

.chat-form input {
  flex: 1;
  padding: 10px;
  border: 1px solid #ccc;
  border-radius: 5px;
}

.chat-form button {
  padding: 10px 15px;
  border: none;
  background-color: #333;
  color: #fff;
  border-radius: 5px;
  margin-left: 10px;
  cursor: pointer;
}

.chat-form button:hover {
  background-color: #555;
}

.message {
  margin-bottom: 10px;
  padding: 5px 10px;
  background-color: #f0f0f0;
  border-radius: 5px;
}

const socket = io();

const form = document.getElementById('chat-form');
const input = document.getElementById('message-input');
const chatBox = document.getElementById('chat-box');

form.addEventListener('submit', function(event) {
  event.preventDefault();
  if (input.value) {
    socket.emit('chat message', input.value);
    input.value = '';
  }
});

socket.on('chat message', function(msg) {
  const item = document.createElement('div');
  item.textContent = msg;
  item.classList.add('message');
  chatBox.appendChild(item);
  chatBox.scrollTop = chatBox.scrollHeight;
});




