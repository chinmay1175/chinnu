# chinnu
https://github.com/chinmay1175/chinnu.git<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Anonymous Chat</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
            height: 100vh;
            margin: 0;
        }
        #chat {
            width: 80%;
            max-width: 600px;
            border: 1px solid #ccc;
            border-radius: 5px;
            padding: 10px;
            background: #f9f9f9;
        }
        #messages {
            height: 300px;
            overflow-y: scroll;
            border-bottom: 1px solid #ccc;
            margin-bottom: 10px;
        }
        #message-input {
            display: flex;
            align-items: center;
        }
        #message {
            flex: 1;
            padding: 10px;
            border: 1px solid #ccc;
            border-radius: 5px;
        }
        #send-button {
            padding: 10px 20px;
            border: none;
            border-radius: 5px;
            background: #007bff;
            color: white;
            cursor: pointer;
        }
    </style>
</head>
<body>
    <div id="chat">
        <div id="messages"></div>
        <div id="message-input">
            <input type="text" id="message" placeholder="Type a message..." />
            <button id="send-button">Send</button>
        </div>
    </div>
    <script>
        const socket = new WebSocket('ws://localhost:3000');

        const messagesDiv = document.getElementById('messages');
        const messageInput = document.getElementById('message');
        const sendButton = document.getElementById('send-button');

        socket.addEventListener('message', function(event) {
            const message = document.createElement('div');
            message.textContent = event.data;
            messagesDiv.appendChild(message);
            messagesDiv.scrollTop = messagesDiv.scrollHeight;
        });

        sendButton.addEventListener('click', function() {
            const message = messageInput.value;
            if (message) {
                socket.send(message);
                messageInput.value = '';
            }
        });

        messageInput.addEventListener('keypress', function(event) {
            if (event.key === 'Enter') {
                sendButton.click();
            }
        });
    </script>
</body>
</html>
Backend (Node.js + WebSocket)
Create a server.js file:

javascript
Copy code
const WebSocket = require('ws');
const http = require('http');

const server = http.createServer();
const wss = new WebSocket.Server({ server });

wss.on('connection', (ws) => {
    ws.on('message', (message) => {
        // Broadcast message to all clients
        wss.clients.forEach((client) => {
            if (client.readyState === WebSocket.OPEN) {
                client.send(message);
            }
        });
    });
});

const PORT = 3000;
server.listen(PORT, () => {
    console.log(`Server is listening on port ${PORT}`);
});
Instructions
Setup Node.js:
