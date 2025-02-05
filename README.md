<!DOCTYPE html>
<html lang="ar">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>شات مشابه لواتساب</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            background-color: #e5ddd5;
            margin: 0;
            padding: 0;
        }

        .chat-container {
            width: 100%;
            max-width: 400px;
            margin: 0 auto;
            height: 100vh;
            display: flex;
            flex-direction: column;
            background-color: #fff;
            border-radius: 10px;
            box-shadow: 0 2px 10px rgba(0, 0, 0, 0.1);
        }

        .chat-header {
            background-color: #075e54;
            color: #fff;
            padding: 15px;
            text-align: center;
            font-size: 20px;
            font-weight: bold;
        }

        .chat-messages {
            flex: 1;
            padding: 20px;
            overflow-y: auto;
            display: flex;
            flex-direction: column-reverse;
        }

        .message {
            max-width: 70%;
            margin: 10px;
            padding: 10px;
            border-radius: 10px;
            position: relative;
            font-size: 14px;
            line-height: 1.5;
        }

        .message.sent {
            background-color: #dcf8c6;
            align-self: flex-end;
        }

        .message.received {
            background-color: #fff;
            align-self: flex-start;
        }

        .message-time {
            font-size: 10px;
            color: #999;
            position: absolute;
            bottom: 5px;
            left: 10px;
        }

        .chat-footer {
            display: flex;
            padding: 10px;
            background-color: #fff;
            border-top: 1px solid #ddd;
        }

        .chat-footer input {
            width: 85%;
            padding: 10px;
            border-radius: 20px;
            border: 1px solid #ddd;
            margin-right: 10px;
        }

        .chat-footer button {
            background-color: #075e54;
            color: #fff;
            border: none;
            border-radius: 50%;
            padding: 10px;
            cursor: pointer;
        }

    </style>
</head>
<body>
    <div class="chat-container">
        <div class="chat-header">
            دردشة مع صديق
        </div>
        <div class="chat-messages" id="messages">
            <!-- الرسائل تظهر هنا -->
        </div>
        <div class="chat-footer">
            <input type="text" id="messageInput" placeholder="اكتب رسالة...">
            <button id="sendButton">➤</button>
        </div>
    </div>

    <script>
        const sendButton = document.getElementById("sendButton");
        const messageInput = document.getElementById("messageInput");
        const messagesContainer = document.getElementById("messages");

        sendButton.addEventListener("click", sendMessage);

        function sendMessage() {
            const messageText = messageInput.value.trim();

            if (messageText !== "") {
                const message = createMessage(messageText, "sent");
                messagesContainer.appendChild(message);
                messageInput.value = ""; // Clear input field
                scrollToBottom();
                setTimeout(() => {
                    const replyMessage = createMessage("تم استلام رسالتك", "received");
                    messagesContainer.appendChild(replyMessage);
                    scrollToBottom();
                }, 1000); // Simulate a reply after 1 second
            }
        }

        function createMessage(text, type) {
            const messageDiv = document.createElement("div");
            messageDiv.classList.add("message", type);

            const messageContent = document.createElement("p");
            messageContent.textContent = text;
            messageDiv.appendChild(messageContent);

            const messageTime = document.createElement("span");
            messageTime.classList.add("message-time");
            const time = new Date();
            messageTime.textContent = `${time.getHours()}:${time.getMinutes()}`;
            messageDiv.appendChild(messageTime);

            return messageDiv;
        }

        function scrollToBottom() {
            messagesContainer.scrollTop = messagesContainer.scrollHeight;
        }
    </script>
</body>
</html>
