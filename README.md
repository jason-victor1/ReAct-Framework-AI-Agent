# Automated AI Agent Using ReAct Framework

## Overview

The **Automated AI Agent Using ReAct Framework** is an advanced AI agent designed to dynamically analyze user inputs, decide when to call external functions, and provide tailored responses. By implementing the ReAct framework, this agent uses a "Thought → Action → Action Response → Final Output" loop to integrate external data and generate dynamic, context-aware answers.

---

## Purpose of the Automated AI Agent

### **Primary Functionality**
- Dynamically answers weather-related questions by calling an external function (`get_weather`) to fetch real-time or simulated weather data.
- Automatically identifies and executes necessary functions based on user input.

### **Key Features**
1. **Dynamic Decision-Making:**
   - Determines when a specific function is needed (e.g., fetching weather data).
   - Implements the ReAct framework loop:  
     *Thought → Action → Action Response → Final Output*.

2. **External Function Integration:**
   - Automatically calls the `get_weather` function with required parameters (e.g., city).

3. **Generalized Workflow:**
   - Handles any weather-related query dynamically, such as:
     - *"What’s the weather in New York?"*  
     - *"Should I take an umbrella in Paris?"*

4. **Educational Value:**
   - Demonstrates how to build a more advanced AI agent with automated decision-making and external function execution.

---

## Example Workflow

### Input:
*"Should I take an umbrella in London?"*

### Process:
1. **Thought:**
   - *"I should check the weather in London."*
2. **Action:**
   - Calls `get_weather` with `city="London"`.
3. **Action Response:**
   - Returns weather data (e.g., `"cloudy"`).
4. **Final Output:**
   - *"The weather in London is cloudy; yes, you should take an umbrella."*

---

## Code: ReAct Framework Automated AI Agent

Save the following code in `ra_with_functions_test.py`:

```python
from openai_module import generate_text_basic
from prompts import react_system_prompt
from sample_functions import get_weather
from json_helpers import extract_json

# Step 1: Define available actions
available_actions = {
    "get_weather": get_weather  # Action for fetching weather
}

# Step 2: User prompt
prompt = """
Should I take an umbrella when going out today in London?
"""

# Step 3: Generate a response with ReAct system prompt
response = generate_text_basic(
    prompt=prompt, 
    model="gpt-4", 
    system_prompt=react_system_prompt
)

# Step 4: Extract the action (if any) from the response
json_function = extract_json(response)

if json_function:
    # Step 5: Parse the function name and parameters
    function_name = json_function[0]['function_name']
    function_parms = json_function[0]['function_parms']

    # Step 6: Validate and execute the function
    if function_name not in available_actions:
        raise Exception(f"Unknown action: {function_name}: {function_parms}")
    
    print(f" -- Running {function_name} with parameters {function_parms}")
    action_function = available_actions[function_name]
    
    # Execute the action and retrieve the result
    result = action_function(**function_parms)
    function_result_message = f"Action_Response: {result}"
    print(function_result_message)
else:
    print("No action detected in the response.")
```

---

## Key Components of the Code

1. **ReAct Framework:**
   - Utilizes the `react_system_prompt` from `prompts.py` to guide the language model.
   - Implements the "Thought → Action → Action Response → Final Output" loop.

2. **Dynamic Action Execution:**
   - Determines whether an action is needed by analyzing the language model's response.

3. **External Functions:**
   - Includes the `get_weather` function from `sample_functions.py`, which fetches weather data.

4. **JSON Parsing:**
   - Extracts structured action details (function name and parameters) from the AI’s response using `extract_json` from `json_helpers.py`.

---

## How the AI Agent Uses the Provided Files
1. **`prompts.py`:**
- Contains the react_system_prompt, which guides the language model's decision-making process.
- Defines the system-level instructions for the ReAct framework.

2. **`ra_test.py`**
- A simplified testing script for validating the ReAct framework agent.
- Provides additional test cases to ensure the agent handles various queries correctly.

3. **`ra_with_functions_test.py`**
- Implements the full ReAct framework logic for the AI agent.
- Handles:
   Dynamic prompt generation.
   Action extraction and execution.
   Final response generation.

4. **`json_helpers.py`**
- Provides a utility function to parse JSON-formatted actions from the language model’s response.

## Critical Files

Make sure the following files are included:

1. **`prompts.py`:**
   - Contains the `react_system_prompt` used to guide the ReAct framework.

2. **`sample_functions.py`:**
   - Defines the `get_weather` function:
     ```python
     def get_weather(city: str):
         if city == "California":
             return "sunny"
         if city == "Paris":
             return "rainy"
         if city == "London":
             return "cloudy"
         if city == "New York":
             return "sunny"
         if city == "Toronto":
             return "rainy"
     ```

3. **`json_helpers.py`:**
   - Provides a utility function to extract JSON-formatted actions from the AI response:
     ```python
     import json

     def extract_json(response):
         try:
             return json.loads(response)
         except ValueError:
             return None
     ```

4. **`openai_module.py`:**
   - Contains the OpenAI API integration for text generation.

5. **`ra_test.py`:**
   - A testing script for validating the ReAct framework agent.

---

## Example Workflow and Output

## Example Workflow

### Input:
*"Should I take an umbrella when going out today in London?"*

### Process:
1. **Thought:**
   - *"I should check the weather in London."*
2. **Action:**
   - Calls `get_weather` with `city="London"`.
3. **Action Response:**
   - The `get_weather` function returns `"cloudy"`.

## Example Output
**Final Output:**
```text
 -- Running get_weather with parameters {'city': 'London'}
Action_Response: Weather in London is cloudy.
```

---

## Key Takeaways

- **Automation:** Automatically identifies and executes required functions based on user input.
- **Dynamic Decision-Making:** Uses the ReAct framework to adapt to various queries.
- **Real-Time Integration:** Integrates with external tools like weather APIs to provide contextually accurate responses.

