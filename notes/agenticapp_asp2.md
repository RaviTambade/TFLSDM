# Multi-Agent Systems and MCP in Practice

Building an AI Agent Team – Connecting LangGraph, MCP and TFLAssessment

Architecture + Python practical lab

Today's theme: One agent can perform a task. Multiple specialized agents can collaborate. MCP gives them a standardized way to access external tools and business capabilities.

## 1. Start with a classroom problem

Mentor: Om, imagine Transflower wants to build an AI-powered student assessment assistant.

A mentor asks:"Analyze this student's performance, identify skill gaps, recommend learning resources, and prepare a progress report."

Can one agent do everything?

Student: Yes, sir. But it may become responsible for too many things.

Mentor: Exactly. In software engineering, we already know the value of separation of responsibilities. We apply the same principle to agent design.

We can create specialized agents:

- Assessment Agent – retrieves and analyzes assessment evidence.
- Skill Agent – identifies skill gaps.
- Learning Agent – recommends learning resources.
- Reporting Agent – prepares the final report.

But remember: specialized agents are not automatically better than ordinary functions. Use multiple agents when independent responsibilities, different tools or separate decision-making genuinely justify them.

## 2. Multi-agent architecture

```
                  MENTOR REQUEST
                        |
                        v
                +----------------+
                | ORCHESTRATOR   |
                | LangGraph      |
                +----------------+
                        |
             +----------+----------+
             |          |          |
             v          v          v
        ASSESSMENT    SKILL     LEARNING
          AGENT       AGENT       AGENT
             |          |          |
             +----------+----------+
                        |
                        v
                 REPORTING AGENT
                        |
                        v
                  VALIDATION
                        |
                        v
                  MENTOR REVIEW
                        |
                        v
                   FINAL REPORT
```

### Responsibility separation

| Agent      | Input                          | Output              |
| ---------- | ------------------------------ | ------------------- |
| Assessment | Assessment records             | Performance summary |
| Skill      | Performance summary + taxonomy | Skill gaps          |
| Learning   | Skill gaps + course resources  | Learning plan       |
| Reporting  | All validated results          | Progress report     |

The orchestrator coordinates the workflow. The agents do not need unrestricted access to one another or to the database.

## 3. Understand MCP in this architecture

Mentor: If our Assessment Agent needs assessment results, should we give it direct MySQL access?

Student: No, sir. It should use authorized business APIs.

Mentor: Correct. But what if different AI applications need to use the same business capabilities? We don't want every AI application to implement a completely different integration.

This is where Model Context Protocol (MCP) becomes useful.

MCP standardizes how an AI application connects to external systems that expose context and capabilities.

### MCP host, client and server

Understand these three roles:

```
+--------------------------------+
| HOST                           |
| AI application / Agent Runtime |
|                                |
|  +--------------------------+  |
|  | MCP CLIENT               |  |
|  | Speaks MCP protocol      |  |
|  +--------------------------+  |
+---------------|----------------+
                |
                | MCP connection
                v
       +--------------------+
       | MCP SERVER         |
       |Exposes capabilities|
       +--------------------+
                |
        +-------+-------+
        |       |       |
        v       v       v
      TOOLS  RESOURCES PROMPTS
```

| MCP capability | Purpose                          | TFL example                 |
| -------------- | -------------------------------- | --------------------------- |
| Tools          | Actions the model can request    | Retrieve assessment results |
| Resources      | Context the application can load | Skill taxonomy              |
| Prompts        | Reusable user-invoked templates  | Generate mentor feedback    |

The distinction matters: tools are model-invoked, resources are application-loaded, and prompts are user-invoked templates.&#x20;

[image](https://www.google.com/s2/favicons?domain=https://github.com\&sz=32)

GitHub+1



## 4. Build a TFLAssessment MCP server

Let's create a small Python project.

```
tfl-mcp-server/
|
+-- server.py
+-- services/
|   +-- assessment_service.py
|   +-- skill_service.py
|
+-- requirements.txt
```

### Step 1 – Install the MCP SDK

The official Python SDK's current stable release line is v2 and requires Python 3.11 or later.&#x20;

[image](https://www.google.com/s2/favicons?domain=https://py.sdk.modelcontextprotocol.io\&sz=32)

MCP Python SDK+1



```
pip install "mcp[cli]"
```

### Step 2 – Create the MCP server

`server.py`

```
from mcp.server import MCPServer

mcp = MCPServer( "TFLAssessment", instructions="Tools for authorized student assessment analysis.")


@mcp.tool()
async def get_assessment_results(student_id: str) -> dict:
    """
    Retrieve assessment results for an authorized student.
    """

    # Teaching placeholder.
    # Production implementation calls the protected
    # TFLAssessment business API.

    return {
        "student_id": student_id,
        "assessments": [
            {"subject": "Python",  "score": 82 },
            {"subject": "SQL", "score": 65},
            {"subject": "API Testing","score": 45}
        ]
    }


@mcp.tool()
async def get_skill_taxonomy() -> dict:
    """
    Retrieve the approved skill taxonomy.
    """

    return {
        "skills": [
            "Python Fundamentals",
            "SQL Queries",
            "API Testing",
            "Problem Solving"
        ]
    }
```

This exposes two tools. In a real implementation, replace the teaching placeholders with API calls and enforce the authenticated user's permissions on every request.

### Step 3 – Run the server

```
uv run mcp dev server.py
```

This starts the development workflow and provides the MCP Inspector for testing tools. For a Streamable HTTP deployment, the official SDK documents this command:

```
uv run mcp run server.py --transport streamable-http
```

The MCP Inspector lets students discover tools, inspect their arguments and test responses.&#x20;

[image](https://www.google.com/s2/favicons?domain=https://py.sdk.modelcontextprotocol.io\&sz=32)

MCP Python SDK+1



## 5. Connect an MCP client to the server

Now let's imagine the Assessment Agent needs to discover and invoke the available tools.

```
import asyncio
from mcp import Client


async def main():

    async with Client("http://localhost:8000/mcp") as client:

        tools = await client.list_tools()

        for tool in tools:
            print(tool.name)

        result = await client.call_tool("get_assessment_results",{"student_id": "STU101"})

        print(result.structured_content)


asyncio.run(main())
```

The MCP client connects to the server, discovers available capabilities, and calls the selected tool. The SDK also supports in-process testing and other transports.&#x20;

[image](https://www.google.com/s2/favicons?domain=https://py.sdk.modelcontextprotocol.io\&sz=32)

MCP Python SDK+1



## 6. Connect MCP tools to LangGraph

Now comes the interesting part.

We have:

- LangGraph – manages workflow.
- MCP Client – discovers and invokes external capabilities.
- MCP Server – exposes TFLAssessment capabilities.
- ASP.NET Core – owns business rules and data access.

One useful approach is to make MCP calls inside specialized LangGraph nodes.

```
                 LANGGRAPH
                     |
             +-------+-------+
             |               |
             v               v
       ASSESSMENT NODE    SKILL NODE
             |               |
             v               v
         MCP CLIENT       MCP CLIENT
             |               |
             +-------+-------+
                     |
                     v
               MCP SERVER
                     |
                     v
             TFLASSESSMENT API
```

### Example: Assessment node

```
from mcp import Client


async def assessment_agent(state):

    async with Client("http://localhost:8000/mcp") as client:

        result = await client.call_tool("get_assessment_results", {"student_id": state["student_id"]})

    return {"assessment_data": result.structured_content}
```

### Example: Skill analysis node

```
async def skill_agent(state):

    assessment_data = state["assessment_data"]

    gaps = []

    for item in assessment_data["assessments"]:

        if item["score"] < 60:
            gaps.append({
                "skill": item["subject"],
                "score": item["score"],
                "status": "Needs Improvement"
            })

    return {
        "skill_gaps": gaps
    }
```

This example uses a deterministic threshold to identify skill gaps. A real evaluation should use the approved assessment rubric, scoring rules and any required minimum evidence. The LLM can explain the findings, but should not invent scores or override the official rules.

## 7. Build the multi-agent workflow

We now connect the specialized nodes.

```

from typing import TypedDict
from langgraph.graph import StateGraph, START, END


class TFLState(TypedDict):
    student_id: str
    assessment_data: dict
    skill_gaps: list
    learning_plan: dict
    report: str


def learning_agent(state):

    gaps = state["skill_gaps"]

    plan = {
        "duration": "7 days",
        "activities": [
            {
                "skill": gap["skill"],
                "activity": f"Practice {gap['skill']} exercises"
            }
            for gap in gaps
        ]
    }

    return {
        "learning_plan": plan
    }


def reporting_agent(state):

    return {
        "report": (
            "Student assessment analysis completed. "
            f"Identified skill gaps: {state['skill_gaps']}"
        )
    }


builder = StateGraph(TFLState)

builder.add_node("assessment", assessment_agent)
builder.add_node("skills", skill_agent)
builder.add_node("learning", learning_agent)
builder.add_node("reporting", reporting_agent)

builder.add_edge(START, "assessment")
builder.add_edge("assessment", "skills")
builder.add_edge("skills", "learning")
builder.add_edge("learning", "reporting")
builder.add_edge("reporting", END)

graph = builder.compile()
```

Run it asynchronously:

```

async def run_workflow():

    result = await graph.ainvoke({
        "student_id": "STU101",
        "assessment_data": {},
        "skill_gaps": [],
        "learning_plan": {},
        "report": ""
    })

    print(result["report"])

```

Here, the nodes represent specialized responsibilities. In a more advanced version, selected nodes can use LLM-powered agents with their own instructions and permitted tools.

Mentor's observation: We have not created four independent LLMs. We have created a coordinated workflow with specialized responsibilities. That is often a better starting architecture.

## 8. Multi-agent communication patterns

There are three common patterns students should understand.

Pattern A – Sequential delegation

One agent's output becomes the next agent's input.

```
Agent A ---> Agent B ---> Agent C
```

Example: Assessment → Skill Analysis → Learning Plan.

Pattern B – Parallel execution

Independent tasks execute simultaneously.

```
            ORCHESTRATOR
             /       \
            v         v
        Agent A     Agent B
            \         /
             v       v
              MERGE
```

Example: Retrieve project history and assessment history at the same time.

Pattern C – Supervisor delegation

A supervisor chooses which specialist should work next.

```
           SUPERVISOR
           /    |    \
          v     v     v
         A      B     C
```

Example: A mentor assistant routes a request to assessment, career or learning specialists.

Choose a pattern based on the dependencies between tasks. Parallelize only independent work, and ensure the supervisor has bounded delegation and execution limits.

## 9. MCP security: The most important production lesson

Mentor: Om, if an agent can call `get_assessment_results`, can it retrieve every student's results?

Student: No, sir. It must only retrieve records the current user is allowed to access.

Mentor: Exactly. Tool discovery is not permission.

```
             USER REQUEST
                  |
                  v
             AI AGENT
                  |
                  v
             MCP CLIENT
                  |
                  v
             MCP SERVER
                  |
                  v
         AUTHENTICATION CHECK
                  |
                  v
         AUTHORIZATION CHECK
                  |
                  v
         BUSINESS API / RULES
                  |
                  v
              DATABASE
```

Production rules:

1. Use trusted MCP servers only.
2. Use least-privilege credentials.
3. Authenticate service-to-service requests.
4. Enforce authorization in the business API.
5. Validate tool arguments and returned data.
6. Require approval for sensitive or consequential operations.
7. Audit tool calls and approval decisions.
8. Never place secrets in prompts, tool descriptions or URLs.

MCP security guidance also emphasizes trusted servers, least privilege and approval for sensitive actions.&#x20;

[image](https://www.google.com/s2/favicons?domain=https://openai.github.io\&sz=32)

OpenAI Agents SDK+1



## 10. Student practical assignment

TAP team challenge

Project: TFLAssessment MCP Server + Multi-Agent Mentor Assistant

Build a working prototype with the following milestones.

| Milestone | Deliverable                                     |
| --------- | ----------------------------------------------- |
| 1         | MCP server exposing assessment and skill tools  |
| 2         | MCP Inspector testing and tool discovery        |
| 3         | LangGraph assessment and skill-analysis nodes   |
| 4         | Learning recommendation agent                   |
| 5         | Reporting agent                                 |
| 6         | Human approval workflow                         |
| 7         | Authentication, authorization and audit logging |
| 8         | Docker deployment and demonstration             |

### Suggested project structure

```
TFLAgenticSystem/
|
+-- frontend/
|   +-- react-chat/
|
+-- backend/
|   +-- TFLAssessment.API/
|
+-- ai-services/
|   +-- agent-service/
|   |   +-- main.py
|   |   +-- graph.py
|   |   +-- state.py
|   |   +-- agents/
|   |   +-- tools/
|   |
|   +-- mcp-server/
|       +-- server.py
|       +-- services/
|
+-- infrastructure/
    +-- docker-compose.yml
```

### Evaluation rubric

| Area                       | Marks |
| -------------------------- | ----- |
| MCP server and tool design | 20    |
| Multi-agent workflow       | 20    |
| LangGraph orchestration    | 15    |
| Business API integration   | 15    |
| Security and authorization | 15    |
| Testing and observability  | 10    |
| Demonstration              | 5     |
| Total                      | 100   |

## 11. Important framework version note

The Python MCP SDK now documents v2 as its current stable release line. Older tutorials using `FastMCP` and v1 imports may not match the v2 API shown in this lesson. Students should follow one version's documentation consistently rather than mixing examples.&#x20;

[image](https://www.google.com/s2/favicons?domain=https://py.sdk.modelcontextprotocol.io\&sz=32)

MCP Python SDK+1



Useful official references:

- [MCP Python SDK – Getting Started](https://py.sdk.modelcontextprotocol.io/get-started/?utm_source=chatgpt.com)
- [MCP – Tools, Resources and Prompts](https://github.com/modelcontextprotocol/python-sdk/blob/main/docs/get-started/first-steps.md?utm_source=chatgpt.com)
- [MCP Python Client](https://py.sdk.modelcontextprotocol.io/client/?utm_source=chatgpt.com)
- [OpenAI Agents SDK – MCP Integration](https://openai.github.io/openai-agents-python/mcp/?utm_source=chatgpt.com)

## 12. Ravi Sir's closing message

Mentor: Om, tell me what we learned.

Student: Sir, LangGraph coordinates workflows, agents perform specialized tasks, and MCP connects those agents to external capabilities.

Mentor: Good. And what protects our business data?

Student: Authentication, authorization, business APIs and governance.

Mentor: Excellent!

> An agent is not an isolated intelligent program. It is a participant in a larger software ecosystem.
>
> Good architecture creates boundaries. MCP creates connectivity. Orchestration creates collaboration. Governance creates trust.

## 13. Next classroom: Agentic AI Testing and Evaluation

Our next engineering question should be:

How do we test a system when an AI agent can choose different tools and different execution paths?

We will explore:

1. Unit testing individual agent nodes.
2. Testing MCP tools independently.
3. Mocking LLM and business API responses.
4. Testing multi-agent workflows.
5. Evaluating hallucinations and grounded responses.
6. Testing unauthorized tool calls.
7. Measuring latency, token usage and execution cost.
8. Building an agent evaluation pipeline using pytest.

That is where students move from building agent demos to engineering reliable agent systems.

🧪Prepare daily agentic AI testing exercisesTap to set up