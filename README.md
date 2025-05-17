## Integration of a Mathematical Calulations with a Chat Completion System using LLM Function-Calling

### AIM:
To design and implement a Python function for calculating the volume of a cylinder, integrate it with a chat completion system utilizing the function-calling feature of a large language model (LLM).

### PROBLEM STATEMENT:

### DESIGN STEPS:
Design and implement a system where:

1.Users interact with a chat interface to calculate the volume of a cylinder.
2.User inputs such as the radius and height of the cylinder are extracted via the LLM.
3.The extracted inputs are passed to a Python function that calculates the volume.
4.The LLM integrates this function and provides the result to the user.

#### STEP 1:
Import necessary libraries, including OpenAI for LLM integration and math for mathematical operations.

#### STEP 2:
Define a Python function to calculate the volume of a cylinder based on its radius and height.
#### STEP 3:
Integrate the function into an LLM-based chat completion system with function-calling capabilities.
### PROGRAM:
```
import math
import os
from dotenv import load_dotenv, find_dotenv
import openai

_ = load_dotenv(find_dotenv())  # read local .env file
openai.api_key = os.environ['OPENAI_API_KEY']

def calculate_cylinder_volume(radius, height): 
    if radius <= 0 or height <= 0:
        return "Enter a valid value."
    
    volume = math.pi * (radius ** 2) * height
    return round(volume, 2)

def chat_with_openai(prompt):
    response = openai.ChatCompletion.create(
        model="gpt-4",
        messages=[
            {"role": "system", "content": "You are an assistant that helps calculate the volume of a cylinder."},
            {"role": "user", "content": prompt},
        ],
        functions=[
            {
                "name": "calculate_cylinder_volume",
                "description": "Calculate the volume of a cylinder given radius and height.",
                "parameters": {
                    "type": "object",
                    "properties": {
                        "radius": {"type": "number", "description": "Radius of the cylinder (in units)"},
                        "height": {"type": "number", "description": "Height of the cylinder (in units)"},
                    },
                    "required": ["radius", "height"],
                },
            }
        ],
        function_call="auto",  
    )
    
    if "function_call" in response["choices"][0]["message"]:
        function_name = response["choices"][0]["message"]["function_call"]["name"]
        arguments = eval(response["choices"][0]["message"]["function_call"]["arguments"])
        if function_name == "calculate_cylinder_volume":
            radius = arguments["radius"]
            height = arguments["height"]
            return calculate_cylinder_volume(radius, height)
    
    return response["choices"][0]["message"]["content"]

radius = float(input("Enter the radius of the cylinder: "))
height = float(input("Enter the height of the cylinder: "))

prompt = f"What is the volume of a cylinder with a radius of {radius} and a height of {height}?"
result = chat_with_openai(prompt)
print("Result:", result)
```

### OUTPUT:
![image](https://github.com/user-attachments/assets/619c1d5b-5e03-4d48-a482-7bc75188080b)


### RESULT:
Hence,the python program to design and implement a Python function for calculating the volume of a cylinder, integrating it with a chat completion system utilizing the function-calling feature of a large language model (LLM) is successfully demonstrated.
