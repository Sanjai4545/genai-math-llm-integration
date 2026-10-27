## Integration of a Mathematical Calulations with a Chat Completion System using LLM Function-Calling

### AIM:
To design and implement a Python function for converting an amount from one currency to another and integrate it with a chat completion system utilizing the function-calling feature of a Large Language Model (LLM).

### PROBLEM STATEMENT:
To develop a Python program that converts an amount from US Dollars (USD) to Indian Rupees (INR) using a predefined exchange rate. The function is integrated with an LLM chat completion system to identify the user's request, call the appropriate function, and display the converted amount.

### DESIGN STEPS:
#### STEP 1: Define a Python function named convert_currency() that accepts the amount, source currency, and target currency as input parameters. Calculate the converted amount using a predefined exchange rate.

#### STEP 2: Define the function schema with its name, description, parameters, and required arguments. Pass the schema and the user's message to the LLM chat completion API to enable function calling.

#### STEP 3: Check whether the LLM requests a function call. If it does, extract the arguments, execute the convert_currency() function, and display the converted amount returned by the function.

### PROGRAM:
```python
import os
import json
import openai
from dotenv import load_dotenv, find_dotenv
load_dotenv(find_dotenv())
if os.environ.get("OPENAI_API_KEY"):
    openai.api_key = os.environ["OPENAI_API_KEY"]
def convert_currency(amount, from_currency, to_currency):
    """Convert an amount between currencies using an example rate."""

    if from_currency.upper() == "USD" and to_currency.upper() == "INR":
        rate = 83.5
    elif from_currency.upper() == "INR" and to_currency.upper() == "USD":
        rate = 1 / 83.5
    elif from_currency.upper() == to_currency.upper():
        rate = 1
    else:
        return json.dumps({
            "error": "Currency pair not supported"
        })

    return json.dumps({
        "amount": amount,
        "from_currency": from_currency.upper(),
        "to_currency": to_currency.upper(),
        "converted_amount": round(amount * rate, 2)
    })

functions = [
    {
        "name": "convert_currency",
        "description": "Convert an amount from one currency to another",
        "parameters": {
            "type": "object",
            "properties": {
                "amount": {
                    "type": "number",
                    "description": "Amount to convert"
                },
                "from_currency": {
                    "type": "string",
                    "description": "Original currency code"
                },
                "to_currency": {
                    "type": "string",
                    "description": "Target currency code"
                }
            },
            "required": [
                "amount",
                "from_currency",
                "to_currency"
            ]
        }
    }
]

messages = [
    {
        "role": "user",
        "content": "Convert 100 USD to INR."
    }
]

response = openai.ChatCompletion.create(
    model="gpt-4o-mini",
    messages=messages,
    functions=functions,
    function_call="auto"
)

message = response["choices"][0]["message"]

if message.get("function_call"):
    function_name = message["function_call"]["name"]
    arguments = json.loads(message["function_call"]["arguments"])

    if function_name == "convert_currency":
        result = convert_currency(**arguments)
        print("Function result:", result)
else:
    print(message["content"])
print("sanjai T")
print("212224240144")
```


### OUTPUT:
<img width="1107" height="75" alt="image" src="https://github.com/user-attachments/assets/91d19266-9074-4942-b8c6-0d62dfc17b28" />



### RESULT:

Thus, the Python function for converting US Dollars (USD) to Indian Rupees (INR) was implemented successfully and integrated with an LLM chat completion system using the function-calling feature.
