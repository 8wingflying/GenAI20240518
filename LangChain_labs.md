# LangChain 實戰


## Streamlit
- [Streamlit • A faster way to build and share data apps](https://streamlit.io/)
- [Streamlit cheat sheet](https://cheat-sheet.streamlit.app/)
- [一个傻瓜式构建可视化 web的 Python 神器 -- streamlit](https://zhuanlan.zhihu.com/p/448853407)

## LABS
- [Build Chatbot Webapp with LangChain](https://www.geeksforgeeks.org/build-chatbot-webapp-with-langchain/)

```
!pip install streamlit 
!pip install OpenAI
!pip install langchain
!pip install WikipediaAPIWrapper
```

```python
from langchain.llms import OpenAI
import os
 
os.environ['OPENAI_API_KEY'] = 'Enter your openAI API key'
model = OpenAI(temperature=0.6)
 
prompt = model("Tell me a joke")
 
print(prompt)
```


```python
from langchain import PromptTemplate
 
template = "{name} is my favourite course at GFG"
 
prompt = PromptTemplate(
    input_variables=["name"],
    template=template,
)
 
example = prompt.format(name="Data Structures and Algorithms")
 
print(example)
```


```python

```

- CHAINS & AGENTS 
```python
from langchain.llms import OpenAI
from langchain.chains import ConversationChain
from langchain.memory import ConversationBufferMemory
import os
os.environ['OPENAI_API_KEY'] = 'Enter your api key'
 
model = OpenAI(temperature=0)
chain = ConversationChain(llm=model,
    verbose=True,
    memory=ConversationBufferMemory()
    )
 
print(chain.predict(input="Can You tell me something about GeeksForGeeks."))
```


```python

```
