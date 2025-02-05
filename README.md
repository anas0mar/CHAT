<!DOCTYPE html>
<html lang="ar">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>شات مشابه لواتساب</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            background-color: #f0f0f0;
            margin: 0;
            padding: 0;
        }

        .chat-container {
            width: 100%;
            max-width: 600px;
            margin: 20px auto;
            background-color: #fff;
            border-radius: 10px;
            overflow: hidden;
            box-shadow: 0 2px 10px rgba(0, 0, 0, 0.1);
        }

        .chat-header {
            background-color: #075e54;
            color: #fff;
            padding: 15px;
            text-align: center;
        }

        .chat-messages {
            padding: 20px;
            height: 400px;
            overflow-y: scroll;
            background-color: #e5ddd5;
        }

        .message {
            max-width: 70%;
            margin: 10px 0;
            padding: 10px;
            border-radius: 10px;
            position: relative;
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
            <h3>الدردشة</h3>
        </div>
        <div class="chat-messages">
            <div class="message received">
                <p>مرحبًا! كيف يمكنني مساعدتك اليوم؟</p>
                <span class="message-time">10:00 AM</span>
            </div>
            <div class="message sent">
                <p>مرحبًا، لدي سؤال حول المنتج.</p>
                <span class="message-time">10:02 AM</span>
            </div>
            <div class="message received">
                <p>بالطبع، ما هو سؤالك؟</p>
                <span class="message-time">10:03 AM</span>
            </div>
        </div>
        <div class="chat-footer">
            <input type="text" placeholder="اكتب رسالة...">
            <button>➤</button>
        </div>
    </div>
</body>
</html>
