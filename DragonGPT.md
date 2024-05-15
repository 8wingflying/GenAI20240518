# 5.[AI 專案開發](AI_Project.md)
  - 打造你的GPT:DragonGPT

![DRAGONGPT.png](DRAGONGPT.png)

## 參考資料
- [LibreChat](LibreChat)
- [chatgpt-clone](https://github.com/xtekky/chatgpt-clone/tree/main)

## 目錄結構 MYCHATGPT_0515
- requirements.txt
- config.py
- app.py
- templates(目錄, 只有一個檔案 index.html
  - index.html 
### 程式碼註解
- config.py
```
API_KEY = "s你CHATGPT的KEYSr"
```
- requirements.txt
```
flask
openai
```
- app.py
```python
"""
file name: app.py
Description: Building a ChatGPT Clone with Flask framework
__copyright__ = "Copyright 2024, MartinYTech"
__author__=  Martin Yanev
__modified__= 03/03/2024
"""

from flask import Flask, request, render_template
import openai
import config

openai.api_key = config.API_KEY
app = Flask(__name__)

conversation_history = []

@app.route("/")
def index():
    return render_template("index.html")


@app.route("/get")
def get_bot_response():
    userText = request.args.get('msg')
    model_engine = "gpt-4o"

    # Append user message to conversation history
    conversation_history.append({"role": "user", "content": userText})
    response = openai.chat.completions.create(
        model=model_engine,
        messages=conversation_history  # Pass the entire conversation history to OpenAI
    )

    # Append AI response to conversation history
    print(response.choices[0].message.content)
    ai_response = response.choices[0].message.content
    conversation_history.append({"role": "assistant", "content": ai_response})

    return ai_response


if __name__ == "__main__":
    app.run()
```

###  首頁 index.html
```html
<!DOCTYPE html>
<html>

<head>
    <title>ChatGPT Chat</title>
    <link rel="stylesheet" href="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.7/css/bootstrap.min.css">
    <style>
        body {
            background-color: #35372D;
            color: #ededf2;
        }

        .container {
            margin-top: 20px;
        }

        #chat {
            height: 400px;
            overflow-y: scroll;
            background-color: #444654;
        }

        .list-group-item {
            border-radius: 5px;
            background-color: #444654;
        }

        .submit {
            background-color: #21232e;
            color: white;
            border-radius: 5px;
        }

        .input-group input {
            background-color: #444654;
            color: #ededf2;
            border: none;
        }
    </style>
</head>

<body>
    <div class="container">
        <h2>我的GPTChat</h2>
        <ul class="nav nav-tabs">
            <li role="presentation">
                <a class="nav-link active" aria-current="page" href="https://www.explainthis.io/zh-hant/chatgpt">ChatGPT提示詞</a>
            </li>
            <li role="presentation">
                <a class="nav-link" href="https://github.com/f/awesome-chatgpt-prompts">Awasom ChatGPT Prompts</a>
            </li>
            <li role="presentation">
                <a class="nav-link" href="https://www.aiprm.com/">AIPRM for ChatGPT 外掛工具</a>
            </li>
            <li role="presentation">
                <a class="nav-link" href="https://www.greataiprompts.com/">GreatAIPrompts</a>
            </li>
            <li role="presentation">
                <a class="nav-link" href="https://library.easyprompt.xyz/">EasyPrompt Library</a>
            </li>
        </ul>
        <hr>
        <div class="panel panel-default">
            <div class="panel-heading">Chat Messages</div>
            <div class="panel-body" id="chat">
                <ul class="list-group">
                </ul>
            </div>
        </div>
        <div class="input-group">
            <input type="text" id="userInput" class="form-control">
            <span class="input-group-btn">
                <button class="btn btn-default" id="submit">Submit</button>
            </span>
        </div>
    </div>
    <script src="https://ajax.googleapis.com/ajax/libs/jquery/3.2.1/jquery.min.js"></script>
    <script>
        $("#submit").click(function () {
            var userInput = $("#userInput").val();
            $.get("/get?msg=" + userInput, function (data) {
                $("#chat").append("<li class='list-group-item'><b>You:</b> " + userInput + "</li>");
                $("#chat").append("<li class='list-group-item'><b>OpenAI:</b> " + data + "</li>");
            });
        });
    </script>
</body>

</html>
```


## 安裝與執行
- pip install -r requirements.txt
- 建目錄如 MYCHATGPT_0515 要有所有檔案
- cd MYCHATGPT_0515
- flask run
- 打開瀏覽器127.0.0.1:5000 ==> 史上最慢的ChatGPT-4o
