# Building Agents with LangChain

## The Most Popular Agent Framework

LangChain is the most widely-used framework for building LLM applications, including agents. Its strength is modularity - you can mix and match components to build custom solutions.

---

## What is LangChain?

LangChain provides abstractions for:

- **Models**: Unified interface to many LLMs
- **Prompts**: Templates and management
- **Chains**: Sequences of operations
- **Agents**: Autonomous reasoning and action
- **Memory**: Conversation and knowledge persistence
- **Tools**: External capabilities

---

## Installation

```bash
# Core package
pip install langchain

# LLM providers
pip install langchain-openai      # For OpenAI
pip install langchain-anthropic   # For Claude
pip install langchain-community   # Community integrations

# Optional: for specific tools
pip install duckduckgo-search    # Web search
pip install wikipedia            # Wikipedia access
```

---

## Quick Start: Simple Agent

```python
from langchain_openai import ChatOpenAI
from langchain.agents import create_react_agent, AgentExecutor
from langchain.tools import DuckDuckGoSearchRun, WikipediaQueryRun
from langchain import hub

# 1. Set up the model
llm = ChatOpenAI(model="gpt-4o", temperature=0)

# 2. Define tools
tools = [
    DuckDuckGoSearchRun(name="web_search"),
    WikipediaQueryRun(name="wikipedia"),
]

# 3. Get a prompt template
prompt = hub.pull("hwchase17/react")

# 4. Create the agent
agent = create_react_agent(llm, tools, prompt)

# 5. Create executor (runs the agent loop)
agent_executor = AgentExecutor(
    agent=agent,
    tools=tools,
    verbose=True,
    handle_parsing_errors=True
)

# 6. Run
result = agent_executor.invoke({
    "input": "What is the population of Tokyo and how does it compare to New York?"
})
print(result["output"])
```

---

## Core Concepts

### Models

LangChain abstracts different LLM providers:

```python
from langchain_openai import ChatOpenAI
from langchain_anthropic import ChatAnthropic

# OpenAI
gpt4 = ChatOpenAI(model="gpt-4o", temperature=0)

# Anthropic
claude = ChatAnthropic(model="claude-3-5-sonnet-20241022")

# Same interface for both
response = gpt4.invoke("Hello!")
response = claude.invoke("Hello!")
```

### Tools

Tools give agents capabilities:

```python
from langchain.tools import tool
from langchain_core.tools import StructuredTool
from pydantic import BaseModel, Field

# Simple function tool
@tool
def calculate(expression: str) -> str:
    """Evaluate a mathematical expression."""
    try:
        return str(eval(expression))
    except Exception as e:
        return f"Error: {e}"

# Structured tool with complex inputs
class SearchInput(BaseModel):
    query: str = Field(description="Search query")
    max_results: int = Field(default=5, description="Max results")

@tool(args_schema=SearchInput)
def search_docs(query: str, max_results: int = 5) -> str:
    """Search internal documentation."""
    # Implementation
    return results
```

### Memory

Persist conversation context:

```python
from langchain.memory import ConversationBufferMemory
from langchain.memory import ConversationSummaryMemory

# Simple buffer - keeps all messages
buffer_memory = ConversationBufferMemory(
    memory_key="chat_history",
    return_messages=True
)

# Summary - compresses old messages
summary_memory = ConversationSummaryMemory(
    llm=llm,
    memory_key="chat_history"
)

# Use with agent
agent_executor = AgentExecutor(
    agent=agent,
    tools=tools,
    memory=buffer_memory,
    verbose=True
)
```

---

## Building Custom Tools

### File Operations Tool

```python
from langchain.tools import tool
from pathlib import Path

@tool
def read_file(path: str) -> str:
    """Read contents of a file.
    
    Args:
        path: Path to the file to read
    
    Returns:
        File contents as string
    """
    try:
        return Path(path).read_text()
    except FileNotFoundError:
        return f"Error: File {path} not found"
    except Exception as e:
        return f"Error reading file: {e}"

@tool
def write_file(path: str, content: str) -> str:
    """Write content to a file.
    
    Args:
        path: Path to write to
        content: Content to write
    
    Returns:
        Success or error message
    """
    try:
        Path(path).write_text(content)
        return f"Successfully wrote to {path}"
    except Exception as e:
        return f"Error writing file: {e}"
```

### Code Execution Tool

```python
import subprocess
from langchain.tools import tool

@tool
def run_python(code: str) -> str:
    """Execute Python code and return output.
    
    Args:
        code: Python code to execute
    
    Returns:
        stdout/stderr from execution
    """
    try:
        result = subprocess.run(
            ["python", "-c", code],
            capture_output=True,
            text=True,
            timeout=30
        )
        output = result.stdout
        if result.stderr:
            output += f"\nErrors:\n{result.stderr}"
        return output or "Code executed successfully (no output)"
    except subprocess.TimeoutExpired:
        return "Error: Code execution timed out"
    except Exception as e:
        return f"Error: {e}"
```

### API Integration Tool

```python
import requests
from langchain.tools import tool
from pydantic import BaseModel, Field

class APIRequestInput(BaseModel):
    url: str = Field(description="API endpoint URL")
    method: str = Field(default="GET", description="HTTP method")
    data: dict = Field(default={}, description="Request body for POST")

@tool(args_schema=APIRequestInput)
def api_request(url: str, method: str = "GET", data: dict = {}) -> str:
    """Make an HTTP request to an API.
    
    Args:
        url: The API endpoint
        method: HTTP method (GET, POST, etc.)
        data: Request body for POST/PUT
    
    Returns:
        API response as string
    """
    try:
        if method.upper() == "GET":
            response = requests.get(url, timeout=10)
        elif method.upper() == "POST":
            response = requests.post(url, json=data, timeout=10)
        else:
            return f"Unsupported method: {method}"
        
        return f"Status: {response.status_code}\n{response.text[:1000]}"
    except Exception as e:
        return f"Error: {e}"
```

---

## Agent Types

### ReAct Agent

Reasoning + Acting (most common):

```python
from langchain.agents import create_react_agent
from langchain import hub

prompt = hub.pull("hwchase17/react")
agent = create_react_agent(llm, tools, prompt)
```

### Structured Chat Agent

For complex tool schemas:

```python
from langchain.agents import create_structured_chat_agent
from langchain import hub

prompt = hub.pull("hwchase17/structured-chat-agent")
agent = create_structured_chat_agent(llm, tools, prompt)
```

### OpenAI Functions Agent

Uses OpenAI's function calling:

```python
from langchain.agents import create_openai_functions_agent
from langchain import hub

prompt = hub.pull("hwchase17/openai-functions-agent")
agent = create_openai_functions_agent(llm, tools, prompt)
```

### Tool Calling Agent

Modern approach using tool calling:

```python
from langchain.agents import create_tool_calling_agent
from langchain_core.prompts import ChatPromptTemplate

prompt = ChatPromptTemplate.from_messages([
    ("system", "You are a helpful assistant."),
    ("human", "{input}"),
    ("placeholder", "{agent_scratchpad}"),
])

agent = create_tool_calling_agent(llm, tools, prompt)
```

---

## Custom Prompts

### Basic Custom Prompt

```python
from langchain_core.prompts import ChatPromptTemplate

system_prompt = """You are a code review assistant.

You have access to these tools:
{tools}

Tool names: {tool_names}

When reviewing code:
1. Check for bugs and logic errors
2. Identify security issues
3. Suggest improvements
4. Be constructive and specific

{agent_scratchpad}
"""

prompt = ChatPromptTemplate.from_messages([
    ("system", system_prompt),
    ("human", "{input}"),
])
```

### Prompt with Examples

```python
from langchain_core.prompts import ChatPromptTemplate, MessagesPlaceholder

prompt = ChatPromptTemplate.from_messages([
    ("system", """You are a helpful coding assistant.
    
Available tools: {tools}
Tool names: {tool_names}"""),
    
    # Few-shot examples
    ("human", "Read the file config.py"),
    ("assistant", "I'll read that file for you."),
    ("tool", "Contents of config.py: DEBUG=True..."),
    ("assistant", "The config.py file contains: DEBUG=True..."),
    
    # Conversation history
    MessagesPlaceholder(variable_name="chat_history"),
    
    # Current input
    ("human", "{input}"),
    MessagesPlaceholder(variable_name="agent_scratchpad"),
])
```

---

## Chains with Agents

### Sequential Chain

```python
from langchain.chains import SequentialChain
from langchain_core.runnables import RunnableSequence

# Plan then execute
planner_prompt = """Create a plan to: {task}
List steps clearly."""

executor_prompt = """Execute this plan:
{plan}

Use tools as needed."""

chain = (
    {"plan": planner_chain}
    | executor_chain
)
```

### Router Chain

Route to different agents:

```python
from langchain_core.runnables import RunnableBranch

router = RunnableBranch(
    (lambda x: "code" in x["input"].lower(), code_agent),
    (lambda x: "search" in x["input"].lower(), search_agent),
    default_agent  # Fallback
)
```

---

## Advanced Patterns

### Self-Correcting Agent

```python
from langchain_core.runnables import RunnableLambda

def validate_output(state):
    """Check if output is valid, re-run if not."""
    if not state.get("output"):
        return {"retry": True}
    if "error" in state["output"].lower():
        return {"retry": True}
    return {"retry": False}

def maybe_retry(state):
    if state["retry"]:
        return agent_executor.invoke(state["input"])
    return state

chain = agent_executor | RunnableLambda(validate_output) | RunnableLambda(maybe_retry)
```

### Parallel Tool Execution

```python
from langchain_core.runnables import RunnableParallel

parallel = RunnableParallel(
    search_results=search_tool,
    file_contents=file_tool,
    api_data=api_tool
)

# All run in parallel
results = parallel.invoke({"query": "test"})
```

### Human-in-the-Loop

```python
from langchain.tools import HumanInputRun

human_input = HumanInputRun()

# Add to tools list
tools = [search, write_file, human_input]

# Agent will ask human when uncertain
```

---

## Error Handling

### Parsing Errors

```python
agent_executor = AgentExecutor(
    agent=agent,
    tools=tools,
    handle_parsing_errors=True,  # Auto-handle malformed responses
    max_iterations=10,           # Prevent infinite loops
    early_stopping_method="generate"  # How to stop
)
```

### Custom Error Handling

```python
from langchain.callbacks import StdOutCallbackHandler

class CustomHandler(StdOutCallbackHandler):
    def on_tool_error(self, error, **kwargs):
        print(f"Tool error: {error}")
        # Log, alert, etc.
    
    def on_agent_error(self, error, **kwargs):
        print(f"Agent error: {error}")

agent_executor = AgentExecutor(
    agent=agent,
    tools=tools,
    callbacks=[CustomHandler()]
)
```

---

## Best Practices

### 1. Clear Tool Descriptions

```python
# Good - agent knows when to use
@tool
def get_weather(city: str) -> str:
    """Get current weather for a city.
    Use when user asks about weather, temperature, or conditions.
    
    Args:
        city: City name (e.g., "New York", "London")
    """

# Bad - too vague
@tool  
def weather(c):
    """Weather function."""
```

### 2. Limit Tool Count

Too many tools confuse the agent. Group related functionality:

```python
# Instead of 10 file tools, use one with actions
@tool
def file_operation(action: str, path: str, content: str = "") -> str:
    """Perform file operations.
    
    Args:
        action: read, write, delete, list
        path: File or directory path
        content: Content for write operations
    """
```

### 3. Set Iteration Limits

```python
agent_executor = AgentExecutor(
    agent=agent,
    tools=tools,
    max_iterations=15,        # Don't run forever
    max_execution_time=120.0  # Timeout in seconds
)
```

### 4. Use Appropriate Models

```python
# Complex reasoning needs strong model
complex_agent = create_react_agent(
    ChatOpenAI(model="gpt-4o"),
    tools, prompt
)

# Simple tasks can use cheaper model
simple_agent = create_react_agent(
    ChatOpenAI(model="gpt-4o-mini"),
    tools, prompt
)
```

---

## Complete Example: Code Review Agent

```python
from langchain_openai import ChatOpenAI
from langchain.agents import create_tool_calling_agent, AgentExecutor
from langchain_core.prompts import ChatPromptTemplate
from langchain.tools import tool
from pathlib import Path

# Tools
@tool
def read_file(path: str) -> str:
    """Read a source code file."""
    try:
        return Path(path).read_text()
    except Exception as e:
        return f"Error: {e}"

@tool
def list_files(directory: str, pattern: str = "*") -> str:
    """List files in a directory."""
    try:
        files = list(Path(directory).glob(pattern))
        return "\n".join(str(f) for f in files[:20])
    except Exception as e:
        return f"Error: {e}"

@tool
def search_code(directory: str, pattern: str) -> str:
    """Search for a pattern in code files."""
    import subprocess
    try:
        result = subprocess.run(
            ["grep", "-r", "-n", pattern, directory],
            capture_output=True, text=True, timeout=10
        )
        return result.stdout[:2000] or "No matches found"
    except Exception as e:
        return f"Error: {e}"

# Setup
llm = ChatOpenAI(model="gpt-4o", temperature=0)
tools = [read_file, list_files, search_code]

prompt = ChatPromptTemplate.from_messages([
    ("system", """You are a code review expert. 
Analyze code for bugs, security issues, and improvements.
Be specific and constructive in feedback.

Available tools: {tools}"""),
    ("human", "{input}"),
    ("placeholder", "{agent_scratchpad}"),
])

agent = create_tool_calling_agent(llm, tools, prompt)
executor = AgentExecutor(agent=agent, tools=tools, verbose=True)

# Run
result = executor.invoke({
    "input": "Review the Python files in ./src for security issues"
})
print(result["output"])
```

---

## Resources

- [LangChain Documentation](https://python.langchain.com/)
- [LangChain Hub](https://smith.langchain.com/hub) - Prompt templates
- [LangSmith](https://smith.langchain.com/) - Debugging and monitoring
- [LangGraph](https://langchain-ai.github.io/langgraph/) - Complex agent workflows

---

*LangChain's strength is flexibility. You can start simple and add complexity as needed. The ecosystem is vast, so focus on the patterns that match your use case rather than trying to learn everything.*

