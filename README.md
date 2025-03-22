## Integration of a Mathematical Calulations with a Chat Completion System using LLM Function-Calling

### AIM:
To design and implement a Python function for calculating the volume of a cylinder, integrate it with a chat completion system utilizing the function-calling feature of a large language model (LLM).

### PROBLEM STATEMENT:

### DESIGN STEPS:

#### STEP 1:
Import necessary libraries, including OpenAI for LLM integration and math for mathematical operations.

#### STEP 2:
Define a Python function to calculate the volume of a cylinder based on its radius and height.
#### STEP 3:
Integrate the function into an LLM-based chat completion system with function-calling capabilities.
### PROGRAM:
```
import os
import openai
```
```
from dotenv import load_dotenv, find_dotenv
_ = load_dotenv(find_dotenv())  # read local .env file
openai.api_key = os.environ['OPENAI_API_KEY']
```
```
from dotenv import load_dotenv, find_dotenv
_ = load_dotenv(find_dotenv())  # read local .env file
openai.api_key = os.environ['OPENAI_API_KEY']
```
```
def chat_with_llm(user_input):
    # OpenAI API call for chat completion
    response = openai.ChatCompletion.create(
        model="gpt-4-0613",  # Function-calling supported model
        messages=[{"role": "user", "content": user_input}],
        functions=[function_schema],
        function_call="auto",  # Let the LLM decide when to use the function
    )

    # Check if a function call is needed
    if response.choices[0].message.get("function_call"):
        function_call = response.choices[0].message["function_call"]
        # Extract arguments
        arguments = eval(function_call["arguments"])  # Use safe parsing methods in production
        radius = arguments["radius"]
        height = arguments["height"]

        # Calculate volume
        volume = calculate_cylinder_volume(radius, height)

        # Send the result back to the LLM for a final response
        response = openai.ChatCompletion.create(
            model="gpt-4-0613",
            messages=[
                {"role": "user", "content": user_input},
                response.choices[0].message,  # Include the function call
                {
                    "role": "function",
                    "name": "calculate_cylinder_volume",
                    "content": str(volume),
                },
            ],
        )
    
    return response.choices[0].message["content"]
```
```

user_input = "What is the volume of a cylinder with radius 3 and height 5?"
output = chat_with_llm(user_input)
print(output)
```
### OUTPUT:
![image](https://github.com/user-attachments/assets/1be3d6a9-4ca2-4484-a791-42fcf684cb19)

### RESULT:
Hence,the python program to design and implement a Python function for calculating the volume of a cylinder, integrating it with a chat completion system utilizing the function-calling feature of a large language model (LLM) is successfully demonstrated.
