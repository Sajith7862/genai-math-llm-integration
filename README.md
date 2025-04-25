## Integration of a Mathematical Calulations with a Chat Completion System using LLM Function-Calling

### AIM:
To design and implement a Python function for calculating the volume of a cylinder, integrate it with a chat completion system utilizing the function-calling feature of a large language model (LLM).

### PROBLEM STATEMENT:

Design and implement a system where a user can input dimensions of a cylinder (radius and height), and the system calculates its volume by invoking a Python function using the function-calling capabilities of an LLM.

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
import math
import json
from dotenv import load_dotenv, find_dotenv

# Load the .env file for API key
_ = load_dotenv(find_dotenv())
openai.api_key = os.environ['OPENAI_API_KEY']

# Define the function for calculating the volume of a cylinder
def calculate_cylinder_volume(radius, height):
    """Calculate the volume of a cylinder."""
    if radius < 0 or height < 0:
        return "Radius and height must be non-negative values."
    volume = math.pi * (radius ** 2) * height
    return round(volume, 2)

# Define the function details
functions = [
    {
        "name": "calculate_cylinder_volume",
        "description": "Calculate the volume of a cylinder given its radius and height.",
        "parameters": {
            "type": "object",
            "properties": {
                "radius": {"type": "number", "description": "Radius of the cylinder's base."},
                "height": {"type": "number", "description": "Height of the cylinder."},
            },
            "required": ["radius", "height"],
        },
    }
]

# Sample conversation messages
messages = [
    {
        "role": "user",
        "content": "What is the volume of a cylinder with radius 5 and height 10?"
    }
]

# Create a chat completion call
response = openai.ChatCompletion.create(
    model="gpt-3.5-turbo",
    messages=messages,
    functions=functions,
    function_call="auto"  # Let the model decide when to call the function
)

# Extract the function arguments from the model's response
function_args = json.loads(response["choices"][0]["message"]["function_call"]["arguments"])
print("Extracted arguments:", function_args)

# Call the function to calculate the volume
result = calculate_cylinder_volume(**function_args)
print("Function result:", result)

# Append the result to the message history
messages.append(
    {
        "role": "function",
        "name": "calculate_cylinder_volume",
        "content": str(result)
    }
)

# Create another response with the updated messages
response = openai.ChatCompletion.create(
    model="gpt-3.5-turbo",
    messages=messages
)

# Print the final response content
print(response["choices"][0]["message"]["content"])

```
### OUTPUT:
![image](https://github.com/user-attachments/assets/367323d0-5630-4578-a2d8-71249fff0e1f)

### RESULT:
Hence,the python program to design and implement a Python function for calculating the volume of a cylinder, integrating it with a chat completion system utilizing the function-calling feature of a large language model (LLM) is successfully demonstrated.
