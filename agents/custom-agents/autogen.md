# Building Agents with AutoGen

## Microsoft's Multi-Agent Framework

AutoGen is Microsoft's framework for building multi-agent systems. Its strength is enabling multiple AI agents to collaborate on complex tasks through conversation.

---

## What is AutoGen?

AutoGen enables:

- **Multi-agent conversations**: Agents talk to each other
- **Human-in-the-loop**: Humans can participate in agent discussions
- **Code execution**: Agents can write and run code
- **Flexible topologies**: Various conversation patterns

### Key Differentiator

While LangChain focuses on chains and single agents, AutoGen excels at **agent-to-agent collaboration**:

```
┌─────────┐     ┌─────────┐     ┌─────────┐
│ Planner │ <-> │ Coder   │ <-> │ Reviewer│
└─────────┘     └─────────┘     └─────────┘
                    │
                    v
              [Code Execution]
```

---

## Installation

```bash
# Core package
pip install pyautogen

# For code execution
pip install docker  # Recommended for sandboxed execution
```

---

## Quick Start: Two-Agent Chat

```python
import autogen

# Configure the LLM
config_list = [
    {
        "model": "gpt-4o",
        "api_key": "your-api-key"
    }
]

llm_config = {"config_list": config_list}

# Create an assistant agent
assistant = autogen.AssistantAgent(
    name="assistant",
    llm_config=llm_config,
    system_message="You are a helpful AI assistant."
)

# Create a user proxy (represents the human)
user_proxy = autogen.UserProxyAgent(
    name="user",
    human_input_mode="TERMINATE",  # Ask human when to stop
    max_consecutive_auto_reply=10,
    code_execution_config={"work_dir": "coding", "use_docker": False}
)

# Start the conversation
user_proxy.initiate_chat(
    assistant,
    message="Create a Python function that calculates compound interest."
)
```

---

## Core Concepts

### Agent Types

#### AssistantAgent

The AI that responds to requests:

```python
assistant = autogen.AssistantAgent(
    name="assistant",
    llm_config=llm_config,
    system_message="""You are a senior software engineer.
    Write clean, well-documented code.
    Always include error handling."""
)
```

#### UserProxyAgent

Represents human or executes code:

```python
user_proxy = autogen.UserProxyAgent(
    name="user",
    human_input_mode="NEVER",  # Fully autonomous
    # human_input_mode="ALWAYS"  # Always ask human
    # human_input_mode="TERMINATE"  # Ask on termination
    
    code_execution_config={
        "work_dir": "output",
        "use_docker": True  # Sandboxed execution
    },
    
    max_consecutive_auto_reply=5
)
```

#### ConversableAgent

Base class for custom agents:

```python
class CustomAgent(autogen.ConversableAgent):
    def generate_reply(self, messages, sender, config):
        # Custom response logic
        return "Custom response"
```

### Code Execution

AutoGen can execute generated code:

```python
user_proxy = autogen.UserProxyAgent(
    name="executor",
    code_execution_config={
        "work_dir": "workspace",
        "use_docker": True,  # Highly recommended
        "timeout": 60,
        "last_n_messages": 3
    }
)

# When assistant generates code, user_proxy executes it
# Results are fed back to continue the conversation
```

---

## Multi-Agent Patterns

### Expert Panel

Multiple specialists collaborate:

```python
# Create specialized agents
planner = autogen.AssistantAgent(
    name="planner",
    llm_config=llm_config,
    system_message="""You are a project planner.
    Break down tasks into clear steps.
    Don't write code, just plan."""
)

coder = autogen.AssistantAgent(
    name="coder",
    llm_config=llm_config,
    system_message="""You are a Python developer.
    Implement code based on the plan.
    Write clean, tested code."""
)

reviewer = autogen.AssistantAgent(
    name="reviewer",
    llm_config=llm_config,
    system_message="""You are a code reviewer.
    Review code for bugs, security, and style.
    Be constructive and specific."""
)

user_proxy = autogen.UserProxyAgent(
    name="user",
    human_input_mode="NEVER",
    code_execution_config={"work_dir": "code"}
)

# Group chat
groupchat = autogen.GroupChat(
    agents=[user_proxy, planner, coder, reviewer],
    messages=[],
    max_round=15
)

manager = autogen.GroupChatManager(
    groupchat=groupchat,
    llm_config=llm_config
)

# Start
user_proxy.initiate_chat(
    manager,
    message="Build a REST API for a todo list application"
)
```

### Teacher-Student

Learning interaction:

```python
teacher = autogen.AssistantAgent(
    name="teacher",
    llm_config=llm_config,
    system_message="""You are a patient programming teacher.
    Explain concepts clearly with examples.
    Ask questions to check understanding.
    Build on what the student knows."""
)

student = autogen.AssistantAgent(
    name="student",
    llm_config=llm_config,
    system_message="""You are learning to program.
    Ask questions when confused.
    Try to solve problems before asking for answers.
    Explain your understanding."""
)

# Student asks teacher
student.initiate_chat(
    teacher,
    message="Can you explain how recursion works?"
)
```

### Debate/Critique

Agents challenge each other:

```python
proposer = autogen.AssistantAgent(
    name="proposer",
    llm_config=llm_config,
    system_message="Propose solutions and defend them."
)

critic = autogen.AssistantAgent(
    name="critic",
    llm_config=llm_config,
    system_message="""Challenge proposed solutions.
    Find weaknesses and edge cases.
    Suggest improvements."""
)
```

---

## Group Chat

### Basic Group Chat

```python
groupchat = autogen.GroupChat(
    agents=[user, assistant1, assistant2],
    messages=[],
    max_round=10,
    speaker_selection_method="auto"  # LLM decides who speaks
    # speaker_selection_method="round_robin"  # Take turns
    # speaker_selection_method="random"  # Random selection
)

manager = autogen.GroupChatManager(
    groupchat=groupchat,
    llm_config=llm_config
)
```

### Custom Speaker Selection

```python
def custom_speaker_selection(last_speaker, groupchat):
    """Define who speaks next based on conversation state."""
    messages = groupchat.messages
    
    # If last message was from planner, coder goes next
    if last_speaker.name == "planner":
        return groupchat.agent_by_name("coder")
    
    # If code was written, reviewer goes next
    if "```python" in messages[-1]["content"]:
        return groupchat.agent_by_name("reviewer")
    
    return "auto"  # Let LLM decide

groupchat = autogen.GroupChat(
    agents=agents,
    messages=[],
    speaker_selection_method=custom_speaker_selection
)
```

---

## Function Calling

### Register Functions

```python
from typing import Annotated

# Define function
def search_web(query: Annotated[str, "Search query"]) -> str:
    """Search the web for information."""
    # Implementation
    return f"Results for: {query}"

def get_weather(
    city: Annotated[str, "City name"],
    units: Annotated[str, "celsius or fahrenheit"] = "celsius"
) -> str:
    """Get weather for a city."""
    # Implementation
    return f"Weather in {city}: 20{units[0]}"

# Register with agent
assistant = autogen.AssistantAgent(
    name="assistant",
    llm_config=llm_config
)

user_proxy = autogen.UserProxyAgent(
    name="user",
    human_input_mode="NEVER"
)

# Register functions
autogen.register_function(
    search_web,
    caller=assistant,
    executor=user_proxy,
    description="Search the web"
)

autogen.register_function(
    get_weather,
    caller=assistant,
    executor=user_proxy,
    description="Get weather information"
)
```

---

## Configuration

### LLM Configuration

```python
# Multiple models with fallback
config_list = [
    {
        "model": "gpt-4o",
        "api_key": os.environ["OPENAI_API_KEY"]
    },
    {
        "model": "gpt-4o-mini",
        "api_key": os.environ["OPENAI_API_KEY"]
    }
]

llm_config = {
    "config_list": config_list,
    "temperature": 0,
    "timeout": 120,
    "cache_seed": 42  # For reproducibility
}
```

### Azure OpenAI

```python
config_list = [
    {
        "model": "gpt-4",
        "api_type": "azure",
        "api_key": os.environ["AZURE_API_KEY"],
        "base_url": "https://your-resource.openai.azure.com",
        "api_version": "2024-02-01"
    }
]
```

### Local Models

```python
# With Ollama
config_list = [
    {
        "model": "codellama",
        "base_url": "http://localhost:11434/v1",
        "api_key": "ollama"  # Required but not used
    }
]
```

---

## Practical Example: Software Development Team

```python
import autogen

config_list = [{"model": "gpt-4o", "api_key": "your-key"}]
llm_config = {"config_list": config_list, "temperature": 0}

# Product Manager - defines requirements
pm = autogen.AssistantAgent(
    name="ProductManager",
    llm_config=llm_config,
    system_message="""You are a product manager.
    Clarify requirements and acceptance criteria.
    Ensure the team builds the right thing.
    Ask clarifying questions when requirements are vague."""
)

# Architect - designs solution
architect = autogen.AssistantAgent(
    name="Architect",
    llm_config=llm_config,
    system_message="""You are a software architect.
    Design system architecture and interfaces.
    Consider scalability, security, and maintainability.
    Create clear technical specifications."""
)

# Developer - implements code
developer = autogen.AssistantAgent(
    name="Developer",
    llm_config=llm_config,
    system_message="""You are a senior developer.
    Write clean, well-documented Python code.
    Follow the architect's design.
    Include error handling and logging."""
)

# QA Engineer - tests code
qa = autogen.AssistantAgent(
    name="QAEngineer",
    llm_config=llm_config,
    system_message="""You are a QA engineer.
    Write comprehensive tests.
    Find edge cases and potential bugs.
    Verify code meets requirements."""
)

# User proxy executes code
user = autogen.UserProxyAgent(
    name="User",
    human_input_mode="TERMINATE",
    code_execution_config={
        "work_dir": "project",
        "use_docker": False
    }
)

# Create group chat
groupchat = autogen.GroupChat(
    agents=[user, pm, architect, developer, qa],
    messages=[],
    max_round=20
)

manager = autogen.GroupChatManager(
    groupchat=groupchat,
    llm_config=llm_config
)

# Start project
user.initiate_chat(
    manager,
    message="""Build a command-line expense tracker that:
    - Lets users add expenses with category and amount
    - Shows spending summary by category
    - Saves data to JSON file
    - Has a simple menu interface"""
)
```

---

## Best Practices

### 1. Clear System Messages

```python
# Good - specific role and constraints
system_message="""You are a security-focused code reviewer.

Your responsibilities:
- Identify security vulnerabilities (SQL injection, XSS, etc.)
- Check authentication and authorization logic
- Review error handling for information leakage
- Verify input validation

Output format:
1. List each issue with severity (HIGH/MEDIUM/LOW)
2. Explain the risk
3. Provide a fix

Do NOT:
- Review code style (that's another agent's job)
- Rewrite entire functions"""

# Bad - vague
system_message="Review the code."
```

### 2. Control Conversation Flow

```python
# Set clear termination conditions
user_proxy = autogen.UserProxyAgent(
    name="user",
    is_termination_msg=lambda x: "TASK COMPLETE" in x.get("content", ""),
    max_consecutive_auto_reply=10
)
```

### 3. Use Docker for Code Execution

```python
code_execution_config={
    "use_docker": True,  # Sandboxed, safe
    "timeout": 60,       # Prevent infinite loops
    "work_dir": "sandbox"
}
```

### 4. Limit Group Chat Rounds

```python
groupchat = autogen.GroupChat(
    agents=agents,
    max_round=15,  # Prevent runaway conversations
    admin_name="user",  # Who can force terminate
    enable_clear_history=True
)
```

---

## Debugging

### Verbose Output

```python
# See all messages
assistant = autogen.AssistantAgent(
    name="assistant",
    llm_config=llm_config
)

# Print conversation
for message in chat_result.chat_history:
    print(f"{message['name']}: {message['content'][:100]}...")
```

### Logging

```python
import logging
logging.basicConfig(level=logging.INFO)

# AutoGen logs will appear
```

### Save Conversation

```python
# After chat
with open("conversation.json", "w") as f:
    import json
    json.dump(groupchat.messages, f, indent=2)
```

---

## Common Patterns

### Iterative Refinement

```python
# Agent keeps improving until satisfactory
while not is_satisfactory(result):
    result = assistant.generate_reply(
        messages=[{"role": "user", "content": f"Improve this: {result}"}]
    )
```

### Consensus Building

```python
# Multiple agents must agree
def check_consensus(messages):
    approvals = sum(1 for m in messages[-3:] if "APPROVED" in m["content"])
    return approvals >= 2

groupchat = autogen.GroupChat(
    agents=[reviewer1, reviewer2, reviewer3],
    is_termination_msg=lambda x: check_consensus(groupchat.messages)
)
```

### Specialized Retrieval

```python
from autogen.agentchat.contrib.retrieve_assistant_agent import RetrieveAssistantAgent
from autogen.agentchat.contrib.retrieve_user_proxy_agent import RetrieveUserProxyAgent

# RAG-enabled agents for document Q&A
assistant = RetrieveAssistantAgent(
    name="assistant",
    llm_config=llm_config
)

ragproxyagent = RetrieveUserProxyAgent(
    name="raguser",
    retrieve_config={
        "task": "qa",
        "docs_path": "./documents"
    }
)
```

---

## Resources

- [AutoGen Documentation](https://microsoft.github.io/autogen/)
- [AutoGen Examples](https://github.com/microsoft/autogen/tree/main/notebook)
- [AutoGen Studio](https://autogen-studio.com/) - No-code agent builder

---

*AutoGen's power is in multi-agent collaboration. When your task benefits from different perspectives, expertise, or iterative refinement, AutoGen provides the framework to make agents work together effectively.*

