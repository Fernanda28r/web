<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <script src="https://cdnjs.cloudflare.com/ajax/libs/crypto-js/4.1.1/crypto-js.min.js"></script>
    <title>Chat de Amlo</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
            height: 100vh;
            margin: 0;
            background-color: #f0f0f0;
        }
        .hidden {
            display: none;
        }
        .centered {
            text-align: center;
        }
        .container {
            width: 90%;
            max-width: 500px;
            background: white;
            padding: 20px;
            border-radius: 8px;
            box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
        }
        #chat {
            border: 1px solid #ccc;
            padding: 10px;
            height: 200px;
            overflow-y: auto;
            margin-bottom: 20px;
        }
        input, textarea {
            width: 100%;
            padding: 10px;
            margin: 10px 0;
            border: 1px solid #ccc;
            border-radius: 4px;
        }
        button {
            padding: 10px 20px;
            background-color: #28a745;
            color: white;
            border: none;
            border-radius: 4px;
            cursor: pointer;
        }
        button:hover {
            background-color: #218838;
        }
        .message {
            margin-bottom: 10px;
        }
        .message strong {
            display: block;
        }
        .encrypted-message {
            font-style: italic;
            color: #666;
        }
    </style>
</head>
<body>
    <div id="login-section" class="container">
        <h1>Iniciar Sesión</h1>
        <label for="name">Nombre:</label>
        <input type="text" id="name" required>
        <button onclick="login()">Iniciar Sesión</button>
    </div>
    
    <div id="welcome-section" class="container hidden">
        <h1>Gracias por iniciar sesión, <span id="username"></span></h1>
        <button onclick="showChat()">Ir al chat</button>
    </div>

    <div id="chat-section" class="container hidden">
        <h1>Chat de Amblo</h1>
        <div id="chat"></div>
        <textarea id="message-input" rows="3" placeholder="Escribe tu mensaje aquí..."></textarea>
        <button id="send-btn">Enviar</button>
    </div>

    <script>
        let userName = '';

        function login() {
            const name = document.getElementById('name').value.trim();
            if (name) {
                userName = name;
                document.getElementById('login-section').classList.add('hidden');
                document.getElementById('username').textContent = userName;
                document.getElementById('welcome-section').classList.remove('hidden');
            } else {
                alert("Por favor, introduce tu nombre.");
            }
        }

        function showChat() {
            document.getElementById('welcome-section').classList.add('hidden');
            document.getElementById('chat-section').classList.remove('hidden');
        }

        document.getElementById('send-btn').addEventListener('click', function() {
            const message = document.getElementById('message-input').value.trim();
            if (message) {
                const encryptedMessage = encryptMessage(message);
                appendMessage(userName, encryptedMessage);
                document.getElementById('message-input').value = '';
            } else {
                alert("Por favor, escribe tu mensaje.");
            }
        });

        function encryptMessage(message) {
            const key = 'claveSuperSecreta123';
            const encrypted = CryptoJS.AES.encrypt(message, key).toString();
            return encrypted;
        }

        function appendMessage(sender, message) {
            const chatBox = document.getElementById('chat');
            const messageElement = document.createElement('div');
            messageElement.classList.add('message');
            messageElement.innerHTML = `<strong>${sender}:</strong> <span class="encrypted-message">${message} (Mensaje cifrado)</span>`;
            chatBox.appendChild(messageElement);
            chatBox.scrollTop = chatBox.scrollHeight;
        }
    </script>
</body>
</html>
