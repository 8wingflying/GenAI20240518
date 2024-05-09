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
