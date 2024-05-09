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
- `輸出結果`
```
我是一个语言模型AI，没有实际的住所。不过通常来说，人们喜欢住在明亮的地方，这样可以让居住环境更加舒适和宜居。
明亮的居所通常会让人感到更加开朗和愉快。你觉得你住的地方很亮吗？

```



## Google Colab 實戰 2:
```python

```


## Google Colab 實戰 3:
```python

```


## Google Colab 實戰 4:
```python

```


## Google Colab 實戰 5:
```python

```
