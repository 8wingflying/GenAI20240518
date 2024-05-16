# OpenAI Programming
- [OpenAI developer platform](https://platform.openai.com/docs/overview)
- [API reference](https://platform.openai.com/docs/api-reference/introduction)

## Google Colab 實戰 1
- 安裝openai ==> !pip install openai

```python
# 載入使用的套件
from openai import OpenAI, OpenAIError # OpenAI 官方套件
import getpass # 保密輸入套件

# 建立 OpenAI 物件

api_key = getpass.getpass("請輸入金鑰：")

client = OpenAI(api_key = api_key) 
```
```
# 建構模型並交談
reply = client.chat.completions.create(
    model = "gpt-3.5-turbo",
    # model = "gpt-4",
    messages = [
        {"role":"user", "content": "你住的地方很亮嗎？"}
    ]
)

```
```python
# 檢視傳回物件

print(reply)
```
`輸出結果`
```
ChatCompletion(
id='chatcmpl-9MriqVT8F3EOWF0LX6OqHY4XeyCAZ',
choices=[
   Choice(finish_reason='stop', index=0, logprobs=None,
          message=ChatCompletionMessage(content='我是一个语言模型AI，没有实际的住所。不过通常来说，人们喜欢住在明亮的地方，这样可以让居住环境更加舒适和宜居。明亮的居所通常会让人感到更加开朗和愉快。你觉得你住的地方很亮吗？',
         role='assistant', function_call=None, tool_calls=None))],

created=1715236752,
model='gpt-3.5-turbo-0125',
object='chat.completion',
system_fingerprint=None,
usage=CompletionUsage(completion_tokens=108, prompt_tokens=21, total_tokens=129)
)
```

```python
## 檢視訊息內容
print(reply.choices[0].message.content)
```
`輸出結果`
```
我是一个语言模型AI，没有实际的住所。不过通常来说，人们喜欢住在明亮的地方，这样可以让居住环境更加舒适和宜居。
明亮的居所通常会让人感到更加开朗和愉快。你觉得你住的地方很亮吗？
```



## Google Colab 實戰 2: 設定 AI 角色
```python
reply = client.chat.completions.create(
    model = "gpt-3.5-turbo",
    messages = [
        {"role":"system", "content":"你是隻住在外太空的猴子"},
        {"role":"user", "content": "你住的地方很亮嗎？ reply in 繁體中文"}
    ]
)

print(reply.choices[0].message.content)
```

`輸出結果`
```
是的，我住的地方非常亮，因為外太空的星星和行星都會照亮我的居所。有時候還能看到漂亮的流星和彗星呢！
```
## Google Colab 實戰 3: 寫成函式
```python
def get_reply(messages):
    try:
        response = client.chat.completions.create(
            model = "gpt-3.5-turbo",
            messages = messages
        )
        reply = response.choices[0].message.content
    except OpenAIError as err:
        reply = f"發生 {err.error.type} 錯誤\n{err.error.message}"
    return reply
```
```python
while True:
    msg = input("你說：")
    if not msg.strip(): break
    messages = [{"role":"user", "content":msg}]
    reply = get_reply(messages)
    print(f"ㄟ唉：{reply}\n")
```
`輸出結果`
```
你說：AI
ㄟ唉：stands for Artificial Intelligence, which refers to the simulation of human intelligence processes by machines, especially computer systems. AI technologies include machine learning, natural language processing, and computer vision. AI is used in a wide range of applications, such as virtual assistants, autonomous vehicles, and medical diagnostics.

你說：BERT
ㄟ唉：BERT (Bidirectional Encoder Representations from Transformers) is a type of neural network architecture designed for natural language processing tasks. It utilizes transformer architecture to learn contextual representations of words in a bidirectional manner, making it highly effective for various NLP tasks such as text classification, named entity recognition, and question answering. BERT has achieved state-of-the-art results on many benchmarks in the field of NLP.
```
## Google Colab 實戰 4: 記憶對話紀錄的函式
```python
hist = []       # 歷史對話紀錄
backtrace = 2   # 記錄幾組對話

def chat(sys_msg, user_msg):
    hist.append({"role":"user", "content":user_msg})
    reply = get_reply(hist
                      + [{"role":"system", "content":sys_msg}])
    while len(hist) >= 2 * backtrace: # 超過記錄限制
        hist.pop(0)                   # 移除最舊紀錄
    hist.append({"role":"assistant", "content":reply})
    return reply
```

`輸出結果`
```
```
## Google Colab 實戰 5: 能接續對話的 AI 程式
```python
sys_msg = input("你希望ㄟ唉扮演：")
if not sys_msg.strip(): sys_msg = '小助理'
print()
while True:
    msg = input("你說：")
    if not msg.strip(): break
    reply = chat(sys_msg, msg)
    print(f"{sys_msg}:{reply}\n")
hist = []
```
`輸出結果`
```
```

## Google Colab 實戰 6:使用google 搜尋套件
```python
# 安裝與匯入 google 搜尋套件

!pip install googlesearch-python
from googlesearch import search

# 使用 google 搜尋套件
for item in search(
    "NBA 2023 冠軍隊", advanced=True, num_results=3):
    print(item.title)
    print(item.description)
    print(item.url)
    print()
```
`輸出結果`
```
```

## Google Colab 實戰 7: 將搜尋結果加入到 content 中
```python
hist = []       # 歷史對話紀錄
backtrace = 2   # 記錄幾組對話

def chat_w(sys_msg, user_msg, search_g = True):
    web_res = []
    if search_g == True: # 代表要搜尋網路
        content = "以下為已發生的事實：\n"
        for res in search(user_msg, advanced=True,
                          num_results=3, lang='zh-TW'):
            content += f"標題：{res.title}\n" \
                       f"摘要：{res.description}\n\n"
        content += "請依照上述事實回答問題 \n"
        web_res = [{"role": "user", "content": content}]
    web_res.append({"role": "user", "content": user_msg})
    while len(hist) >= 2 * backtrace: # 超過記錄限制
        hist.pop(0)  # 移除最舊的紀錄
    reply_full = ""
    for reply in get_reply(
        hist                          # 先提供歷史紀錄
        + web_res                     # 再提供搜尋結果及目前訊息
        + [{"role": "system", "content": sys_msg}]):
        reply_full += reply           # 記錄到目前為止收到的訊息
        yield reply                   # 傳回本次收到的片段訊息
    hist.append({"role": "user", "content": user_msg})
    while len(hist) >= 2 * backtrace: # 超過記錄限制
        hist.pop(0)                   # 移除最舊紀錄
    hist.append({"role":"assistant", "content":reply_full})

sys_msg = '小助理'

while True:
    msg = input("你說：")
    if not msg.strip(): break
    print(f"{sys_msg}：", end = "")
    for reply in chat_w(sys_msg, msg, search_g = True):
        print(reply, end = "")
    print('\n')
hist = []
```
`輸出結果`
```
```
