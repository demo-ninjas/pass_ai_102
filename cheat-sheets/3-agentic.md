# Cheat Sheet 3: Implement an Agentic Solution (5–10%)

> Smallest domain but easy points if you know the concepts.

---

## What Is an AI Agent?

An AI agent is an autonomous system that:
1. **Receives** a goal or task
2. **Plans** steps to accomplish it
3. **Uses tools** (APIs, functions, data sources) to take actions
4. **Iterates** — observes results and adjusts
5. **Returns** a final answer or outcome

### Agent vs Chatbot
| | Chatbot | Agent |
|-|---------|-------|
| **Autonomy** | Responds to prompts | Plans and acts independently |
| **Tools** | No external tool use | Calls APIs, searches, runs code |
| **Memory** | Stateless or simple history | Maintains state across steps |
| **Iteration** | Single turn | Multi-step reasoning loops |

---

## Microsoft Foundry Agent Service

| Aspect | Detail |
|--------|--------|
| **What** | Managed service for building AI agents in Microsoft Foundry |
| **Built on** | Azure OpenAI + tool integrations |
| **Key feature** | Declarative agent definition (no code / low code) |
| **Tools** | Code interpreter, file search, function calling, Azure AI Search |

### Built-in Agent Tools
| Tool | What It Does |
|------|--------------|
| **Code Interpreter** | Executes Python code in a sandbox (data analysis, charts, math) |
| **File Search** | Searches uploaded files using vector retrieval |
| **Function Calling** | Calls your custom functions/APIs |
| **Azure AI Search** | Queries a search index (RAG) |
| **Bing Search** | Web search grounding |
| **Azure Functions** | Run serverless code |
| **Microsoft 365** | Access emails, calendar, files (with Graph) |

### Agent Service API Flow
```
1. Create an Agent (model + instructions + tools)
2. Create a Thread (conversation)
3. Add Messages to Thread (user input)
4. Create a Run (agent processes the thread)
5. Poll Run status → retrieve results
```

### Agent API Key Objects
| Object | Purpose |
|--------|---------|
| `Agent` | Definition: model, instructions, tools |
| `Thread` | Conversation container; holds messages |
| `Message` | A single message (user or assistant) |
| `Run` | An execution of the agent on a thread |
| `RunStep` | Individual step within a run (tool call, message creation) |

---

## Microsoft Agent Framework (Semantic Kernel / AutoGen)

| Framework | Description |
|-----------|-------------|
| **Semantic Kernel** | SDK (.NET, Python, Java) for building AI agents with plugins and planners |
| **AutoGen** | Framework for multi-agent conversations and autonomous workflows |

### Semantic Kernel Key Concepts
| Concept | What It Is |
|---------|------------|
| **Kernel** | Central orchestrator; holds services, plugins, memory |
| **Plugin** | Collection of functions the agent can call |
| **Function** | A single callable action (native code or prompt) |
| **Planner** | Automatically creates a plan from available functions |
| **Memory** | Stores conversation history and facts |
| **Connector** | Links to AI services (OpenAI, Search, etc.) |

---

## Multi-Agent Orchestration

### Patterns
| Pattern | Description |
|---------|-------------|
| **Sequential** | Agents execute in a fixed order; output of one feeds the next |
| **Parallel** | Multiple agents work simultaneously on subtasks |
| **Hierarchical** | Manager agent delegates to worker agents |
| **Collaborative** | Agents discuss and refine answers together |

### Multi-Agent Key Considerations
- **Handoff** — How agents transfer control to each other
- **Shared state** — How agents share context/memory
- **Conflict resolution** — How contradictions between agents are handled
- **Human-in-the-loop** — When to involve a human for approval

---

## Testing, Optimizing, and Deploying Agents

### Testing
- Unit test individual tools/functions
- End-to-end test agent flows with test cases
- Evaluate with ground truth (expected vs actual outcomes)
- Test edge cases and adversarial inputs

### Optimization
- Refine system instructions for clarity
- Optimize tool descriptions (agent picks tools based on descriptions)
- Reduce unnecessary tool calls
- Use smaller/faster models for simple subtasks

### Deployment
- Deploy via Microsoft Foundry portal or SDK
- Monitor with Application Insights / tracing
- Set up content safety filters
- Implement rate limiting and authentication
