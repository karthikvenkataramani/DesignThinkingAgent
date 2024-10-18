### Summary:

This Python code implements a **Design Thinking AI Agent** using LangChain and Groq's LLM (Llama3-8b-8192). The agent consists of five sub-agents, each specializing in a stage of the design thinking process: Empathize, Define, Ideate, Prototype, and Test. It utilizes Groq's large language model (LLM) via the `ChatGroq` class to generate contextually relevant responses and solutions based on the user input, following the design thinking methodology.

The code sequentially runs the design thinking process, from gathering user personas and formulating problem statements to generating ideas, prototyping solutions, and testing those solutions. The results are presented in a user-friendly format, and the user can refine each step interactively.

### Main Components:

1. **Libraries and Groq API Setup**:
   - Several libraries are imported, including LangChain and `ChatGroq` for interacting with Groq's LLM.
   - The Groq API key is retrieved from Google Colab's user data and used to instantiate the `ChatGroq` object.

2. **DesignThinkingAgent Class**:
   - This class defines the core behavior of each agent in the design thinking process.
   - Each agent generates a specific prompt based on its role and uses Groq's LLM to generate responses.
   - The stages include Empathize (user personas), Define (problem statements), Ideate (solution ideas), Prototype (solution description), and Test (evaluation).

3. **Main Execution**:
   - The user is prompted to input their vision or problem statement.
   - The design thinking process is carried out sequentially, with each stage generating JSON-formatted output.
   - The results (personas, problem statements, ideas, prototype, and evaluation) are printed, and the user is asked if they want to refine any stage.

4. **Refinement Loop**:
   - The code allows the user to refine any of the stages by rerunning the associated agent with the original vision.
   - Results from each refinement are displayed, allowing for iterative improvements.

5. **Utility Functions**:
   - `parse_json`: Safely parses JSON responses from the LLM, handling errors if they arise.

---

### Code Documentation:

#### 1. **Library Installations and Setup:**
   ```python
   !pip install langchain langchain_community langchain-groq
   ```
   Installs necessary packages for using LangChain, Groq's LLM, and LangChain Community tools.

#### 2. **Groq API Key and Model Initialization:**
   ```python
   from google.colab import userdata
   groq_api_key = userdata.get('GROQ_API_KEY')
   if groq_api_key is None:
       raise ValueError("GROQ_API_KEY not found in Google Colab secrets.")
   groq_llm = ChatGroq(model="llama3-8b-8192", api_key=groq_api_key)
   ```
   - Retrieves the Groq API key from Colab's stored user data and initializes the Groq LLM using the `ChatGroq` class.

#### 3. **DesignThinkingAgent Class:**
   ```python
   class DesignThinkingAgent:
       def __init__(self, name):
           self.name = name
           self.state = {}
           self.llm_tool = LLMChain(llm=groq_llm, prompt=PromptTemplate.from_template("{input}"))
   ```
   - This class defines an agent responsible for a specific design thinking stage. The `run()` method executes the agent's prompt.

   - **Methods**:
     - `run(input)`: Processes user input and generates a response by calling `handle_message()`.
     - `handle_message(message)`: Builds the LLM prompt based on the agent’s stage and generates a response.
     - `get_stage_prompt()`: Provides the relevant prompt template for each design thinking stage.

#### 4. **Agent Instances:**
   ```python
   empathize_agent = DesignThinkingAgent("Empathize Agent")
   define_agent = DesignThinkingAgent("Define Agent")
   ideate_agent = DesignThinkingAgent("Ideate Agent")
   prototype_agent = DesignThinkingAgent("Prototype Agent")
   test_agent = DesignThinkingAgent("Test Agent")
   ```
   - Instances of the `DesignThinkingAgent` class are created for each design thinking stage.

#### 5. **Main Execution:**
   ```python
   vision = input("Please enter your vision or problem statement: ")
   personas_raw = empathize_agent.run(vision)
   personas = parse_json(personas_raw)
   ```
   - The user provides input (vision/problem statement), and each agent is run in sequence to perform the design thinking process. Results are parsed and printed.

#### 6. **Refinement Loop:**
   ```python
   while True:
       user_input = input("\nDo you want to refine any part of the process? (empathize/define/ideate/prototype/test/no): ").lower()
       if user_input == "no":
           break
   ```
   - After the main execution, the user can interactively refine any stage by rerunning the corresponding agent with the same input.

#### 7. **Result Parsing:**
   ```python
   def parse_json(json_str):
       try:
           return json.loads(json_str)
       except json.JSONDecodeError:
           print(f"Error parsing JSON. Raw output: {json_str}")
           return None
   ```
   - A helper function to safely parse the JSON responses generated by the agents, printing an error message in case of parsing issues.

---

This code efficiently guides users through the design thinking process using a conversational agent powered by Groq's LLM, allowing for an interactive, iterative approach to refining ideas and solutions.

<img width="1440" alt="Screenshot 2024-10-19 at 1 17 38 AM" src="https://github.com/user-attachments/assets/e3c9785e-2f69-4aba-85ba-0037fe4825ef">
<img width="1186" alt="Screenshot 2024-10-19 at 1 18 08 AM" src="https://github.com/user-attachments/assets/7c3fb9a7-f17b-4b69-9f3f-7e0fcc5cf2ea">
<img width="1328" alt="Screenshot 2024-10-19 at 1 18 25 AM" src="https://github.com/user-attachments/assets/3fbc19ce-7ebb-4b90-afc8-fd8cbf079b8b">
<img width="1338" alt="Screenshot 2024-10-19 at 1 18 35 AM" src="https://github.com/user-attachments/assets/7926fb30-a236-46e2-b1b1-a5a3a0d967a7">
<img width="1320" alt="Screenshot 2024-10-19 at 1 18 58 AM" src="https://github.com/user-attachments/assets/6d15cfb3-ffc1-4e4c-83c2-3720dafdcfac">
<img width="1337" alt="Screenshot 2024-10-19 at 1 20 31 AM" src="https://github.com/user-attachments/assets/118dfc89-e912-4eb6-9631-431007614292">
<img width="1367" alt="Screenshot 2024-10-19 at 1 20 46 AM" src="https://github.com/user-attachments/assets/cc82265d-886a-46d5-a04b-53fafb76b534">




