# Usage

1. Start a **llama.cpp** server via the `./server` command.
2. Run the **llama.cpp**'s OpenAI-like server.

```
python ./examples/server/api_like_OAI.py
```

4. Use OpenAI's API in Python like this:

```python
from openai import OpenAI

# Modify OpenAI's API key and API base
openai_api_key = "whatever"
openai_api_base = "http://localhost:8081" # --> the host:port of llama.cpp's OpenAI-like server

client = OpenAI(
    # defaults to os.environ.get("OPENAI_API_KEY")
    api_key=openai_api_key,
    base_url=openai_api_base,
)

# models = client.models.list() # --> No support for this on llama.cpp yet
# model = models.data[0].id

chat_completion = client.chat.completions.create(
    messages=[{
        "role": "system",
        "content": "You are a helpful assistant."
    }, {
        "role": "user",
        "content": "Which plan is best? Plan A or Plan B? Write only the plan's letter."
    }
    ],
    # model=model,
    model="whatever",
    # for additional arguments such as `grammar` that llama.cpp accepts but OpenAI doesn't, write them in `extra_body`:
    extra_body={
        # "grammar": '''root ::= Template\nTemplate ::= options \noptions ::= ("A" | "B")''',
    }
)

print(chat_completion.choices[0].message.content)
```

# Why not just use `requests` and send POST requests?

Because using OpenAI's package provides type and completion hints which your IDE can use to help you avoid making mistakes. But when you send POST requests (using the `requests` library), you enter them as a dictionary and the IDE can't validate it.
