# Building a Production-Ready AI Agent

LangGraph + ASP.NET Core – From Agent Design to Implementation

Architecture + practical lab

Today's objective: Build an AI agent that can understand a student's request, call authorized business APIs, maintain workflow state, generate a learning recommendation, and ask for mentor approval when necessary.

## 1. Start with a real-world problem

Mentor: Om, imagine a student asks TFLCoMentor: "Analyze my recent assessment results and prepare a 7-day improvement plan." Should we ask the LLM to invent a plan based on its general knowledge?

Student: No, sir. First, it should retrieve the student's actual assessment results.

Mentor: Correct. And where should those results come from?

Student: From our Assessment API, sir.

Mentor: Excellent. The model can help interpret evidence, but the business system remains the source of truth.

Our engineering goal is therefore:

- Retrieve real student data.
- Retrieve trusted learning resources.
- Analyze skill gaps.
- Generate a structured plan.
- Validate the plan.
- Request approval where required.
- Record the outcome.

## 2. Production architecture

We will separate the business application from the AI agent runtime.

```
                 STUDENT / MENTOR
                         |
                         v
                  REACT WEB APP
                         |
                         v
                  ASP.NET CORE API
              +----------------------+
              | JWT Authentication   |
              | RBAC Authorization   |
              | Input Validation     |
              | Rate Limiting        |
              +----------------------+
                         |
                         v
                  AGENT SERVICE
                  Python + FastAPI
                         |
                         v
                  LANGGRAPH RUNTIME
              +----------------------+
              | Workflow State       |
              | Agent Nodes          |
              | Conditional Edges    |
              | Checkpointing        |
              +----------------------+
                         |
             +-----------+-----------+
             |           |           |
             v           v           v
          PROFILE     ASSESSMENT   KNOWLEDGE
           TOOL         TOOL         TOOL
             |           |           |
             v           v           v
          ASP.NET     ASP.NET      RAG SERVICE
          PROFILE     ASSESSMENT   VECTOR STORE
             |           |           |
             +-----------+-----------+
                         |
                         v
                  LLM / MODEL
                         |
                         v
                  VALIDATION
                         |
                         v
                  MENTOR APPROVAL
                         |
                         v
                  LEARNING PLAN
```

Important design decision: ASP.NET Core remains responsible for authentication, authorization and business rules. LangGraph manages the AI workflow. Python and FastAPI host the agent service in this teaching architecture.

## 3. What is LangGraph?

LangGraph : A framework for building stateful, multi-step agent workflows.

Think of LangGraph as a workflow engine in which nodes perform work and edges determine what happens next.

Four basic concepts:

| Concept          | Meaning                  | Example                   |
| ---------------- | ------------------------ | ------------------------- |
| State            | Shared workflow data     | Student ID, results, plan |
| Node             | A unit of execution      | Retrieve assessment       |
| Edge             | Transition between nodes | Retrieval → Analysis      |
| Conditional edge | Decision-based routing   | Approve or request review |

### Graph-based workflow

```
                  START
                    |
                    v
              LOAD PROFILE
                    |
                    v
            FETCH ASSESSMENTS
                    |
                    v
             ANALYZE SKILLS
                    |
                    v
             GENERATE PLAN
                    |
                    v
              VALIDATE PLAN
                    |
             +------+------+
             |             |
          VALID         INVALID
             |             |
             v             v
        MENTOR REVIEW   REVISE PLAN
             |             |
       +-----+-----+       |
       |           |       |
    APPROVE      REJECT    |
       |           |       |
       v           v       |
     PUBLISH      STOP     |
       |                   |
       v                   |
      END <----------------+
```

## 4. Design the agent state first

Mentor: Before writing nodes, what information must flow through our graph?

Student: Student ID, assessment results, skill gaps and the learning plan.

Mentor: Exactly. That information becomes the workflow state.

A simplified Python implementation:

```
from typing import TypedDict, Optional


class StudentAgentState(TypedDict):
    student_id: str
    request: str

    profile: Optional[dict]
    assessments: Optional[list]

    skill_gaps: Optional[list]
    learning_plan: Optional[dict]

    validation_status: Optional[str]
    mentor_approval: Optional[bool]

    messages: list[str]

```

Think of state as a shared notebook passed from one node to another.

```
        GRAPH STATE
   +----------------------+
   | student_id           |
   | request              |
   | profile              |
   | assessments          |
   | skill_gaps           |
   | learning_plan        |
   | validation_status    |
   | mentor_approval      |
   +----------------------+
          |
          v
   Passed between nodes
```

Engineering principle: Keep workflow state structured. Don't store everything in one unstructured prompt.

## 5. Build the graph nodes

Each node should perform one clearly defined responsibility.

```
def load_profile(state: StudentAgentState):
    # Call authorized Student Profile API
    profile = get_student_profile(state["student_id"])

    return {
        "profile": profile,
        "messages": ["Student profile retrieved"]
    }


def fetch_assessments(state: StudentAgentState):
    assessments = get_assessment_results(state["student_id"]
    )

    return {
        "assessments": assessments,
        "messages": ["Assessment results retrieved"]
    }


def analyze_skills(state: StudentAgentState):
    # Analyze actual assessment evidence
    skill_gaps = analyze_assessment_data(state["assessments"])

    return {
        "skill_gaps": skill_gaps,
        "messages": ["Skill gaps identified"]
    }


def generate_plan(state: StudentAgentState):
    plan = create_learning_plan(profile=state["profile"],skill_gaps=state["skill_gaps"])

    return {
        "learning_plan": plan,
        "messages": ["Learning plan generated"]
    }


def validate_plan(state: StudentAgentState):
    plan = state["learning_plan"]

    status = (
        "valid"
        if validate_learning_plan(plan)
        else "invalid"
    )

    return {
        "validation_status": status
    }
    
```

These functions illustrate the responsibilities. The API clients, model calls, schema validation and error handling must be implemented before treating them as production code.

## 6. Connect the nodes using LangGraph

Now we define the workflow.

```

from langgraph.graph import StateGraph, START, END

builder = StateGraph(StudentAgentState)

builder.add_node("profile", load_profile)
builder.add_node("assessments", fetch_assessments)
builder.add_node("analysis", analyze_skills)
builder.add_node("plan", generate_plan)
builder.add_node("validation", validate_plan)

builder.add_edge(START, "profile")
builder.add_edge("profile", "assessments")
builder.add_edge("assessments", "analysis")
builder.add_edge("analysis", "plan")
builder.add_edge("plan", "validation")

builder.add_conditional_edges(
    "validation",
    lambda state: state["validation_status"],
    {
        "valid": END,
        "invalid": "plan"
    }
)

graph = builder.compile()

```

The graph now has a sequence of responsibilities and a conditional transition.

For an actual application, add a bounded retry count to the state. Otherwise, an invalid plan could send the graph into an endless loop.

### Execute the graph

```

initial_state = {
    "student_id": "STU101",
    "request": "Prepare a 7-day improvement plan",
    "profile": None,
    "assessments": None,
    "skill_gaps": None,
    "learning_plan": None,
    "validation_status": None,
    "mentor_approval": None,
    "messages": []
}

result = graph.invoke(initial_state)

print(result["learning_plan"])


```

Teaching example: API implementations, authorization context, model configuration and checkpoint persistence are intentionally simplified.

## 7. Integrate the agent with ASP.NET Core

Now let's connect the Python agent service to our existing .NET backend.

Why not put everything into one application?

We can, but separate services let the team independently manage the business API and the agent runtime.

### ASP.NET Core calls the agent

```
public class AgentClient
{
    private readonly HttpClient _httpClient;

    public AgentClient(HttpClient httpClient)
    {
        _httpClient = httpClient;
    }

    public async Task<string> GeneratePlanAsync( string studentId,CancellationToken cancellationToken)
    {
        var response = await _httpClient.PostAsJsonAsync("/agent/learning-plan",new { studentId }, cancellationToken);

        response.EnsureSuccessStatusCode();

        return await response.Content.ReadAsStringAsync( cancellationToken );
    }
}
```

Register the client:

```
builder.Services.AddHttpClient<AgentClient>(client => {  client.BaseAddress = new Uri("http://agent-service:8000"); });
```

Expose a backend endpoint:

```
app.MapPost(
    "/api/students/{studentId}/learning-plan",
    async (string studentId, AgentClient agent, CancellationToken cancellationToken) =>
    {
        var result = await agent.GeneratePlanAsync(studentId,cancellationToken);

        return Results.Ok(result);
    }
).RequireAuthorization();
```

This is an integration skeleton, not a complete production security implementation. The authenticated identity must be checked against the requested student ID. The agent service must also receive a trusted, scoped authorization context rather than accepting arbitrary student IDs from the browser.

## 8. Expose the agent through FastAPI

```
from fastapi import FastAPI
from pydantic import BaseModel

app = FastAPI()


class LearningPlanRequest(BaseModel):
    studentId: str


@app.post("/agent/learning-plan")
def generate_learning_plan(
    request: LearningPlanRequest
):
    initial_state = {
        "student_id": request.studentId,
        "request": "Prepare a 7-day learning plan",
        "profile": None,
        "assessments": None,
        "skill_gaps": None,
        "learning_plan": None,
        "validation_status": None,
        "mentor_approval": None,
        "messages": []
    }

    result = graph.invoke(initial_state)

    return {
        "studentId": request.studentId,
        "learningPlan": result["learning_plan"],
        "validationStatus": result["validation_status"]
    }

```

For production, this endpoint should be asynchronous where appropriate, authenticate service-to-service requests, validate input, enforce timeouts, and return a typed response model.

## 9. Add human approval using checkpoints

This is where an ordinary workflow becomes a controlled agent workflow. Suppose the agent generates a learning plan. The mentor must approve it before it is published.

```
          AGENT GENERATES PLAN
                    |
                    v
             VALIDATE PLAN
                    |
                    v
             SAVE WORKFLOW STATE
                    |
                    v
             WAIT FOR MENTOR
                    |
             +------+------+
             |             |
          APPROVE        REJECT
             |             |
             v             v
          PUBLISH       REVISE
             |             |
             v             v
            END       VALIDATION
```

LangGraph supports checkpointing and human-in-the-loop patterns. In an implementation, you can:

1. Configure a persistent checkpointer.
2. Give each workflow a stable thread identifier.
3. Pause execution at an approval boundary.
4. Present the proposed plan to the mentor.
5. Resume with the mentor's decision.
6. Record who approved, when, and what was approved.

A critical distinction: a workflow pause is not an authorization mechanism. The backend must verify the mentor's identity and approval permissions before accepting the decision.

## 10. Production engineering checklist

A working demo is not a production-ready agent. Let's examine what else is required.

| Engineering area  | What to implement                        |
| ----------------- | ---------------------------------------- |
| Authentication    | JWT / service identity                   |
| Authorization     | Student and mentor access boundaries     |
| State persistence | Durable graph checkpoints                |
| Reliability       | Retry limits, timeouts, circuit breakers |
| Validation        | Pydantic schemas and business rules      |
| Security          | Secret management, input validation      |
| Observability     | Traces, logs, token and latency metrics  |
| Evaluation        | Test datasets and expected outcomes      |
| Governance        | Approval and audit history               |
| Deployment        | Docker, CI/CD and health checks          |

### Deployment architecture

```
                 DOCKER ENVIRONMENT
+--------------------------------------------------+
|                                                  |
|  +------------------+   +---------------------+  |
|  | React Frontend   |   | ASP.NET Core API    |  |
|  +------------------+   +---------------------+  |
|                                  |               |
|                                  v               |
|                         +------------------+     |
|                         | Python Agent     |     |
|                         | FastAPI          |     |
|                         +------------------+     |
|                                  |               |
|                +-----------------+----------+    |
|                |                 |          |    |
|                v                 v          v    |
|            MySQL DB          Vector DB   LLM API |
|                                                  |
+--------------------------------------------------+
```

Remember: MySQL stores business records. A vector database supports semantic retrieval. They serve different purposes.

## 11. Practical assignment for TAP students

Team implementation challenge

Project: TFL Student Learning Assistant

Build the system in four milestones.

1. Milestone 1 – **Business API**
   - Create Student Profile API.
   - Create Assessment Results API.
   - Add JWT authentication and RBAC.
   - Test endpoints using Postman.
2. Milestone 2 – **Agent workflow**
   - Create FastAPI project.
   - Define LangGraph state.
   - Implement profile and assessment nodes.
   - Generate a structured learning plan.
3. Milestone 3 – **Human approval**
   - Add mentor approval.
   - Persist workflow state.
   - Add validation and bounded retries.
   - Record approval audit events.
4. Milestone 4 – **Production readiness**
   - Dockerize the services.
   - Add tracing and structured logs.
   - Evaluate at least 20 test scenarios.
   - Demonstrate failure handling.

### Student evaluation

| Criteria                     | Marks |
| ---------------------------- | ----- |
| Business API design          | 15    |
| LangGraph state and nodes    | 20    |
| Tool integration             | 15    |
| Workflow and checkpointing   | 15    |
| Security and authorization   | 15    |
| Testing and evaluation       | 10    |
| Deployment and demonstration | 10    |
| Total                        | 100   |

## 12. Mentor's final classroom discussion

Mentor: Om, what did we build today?

Student: Sir, we designed an AI agent workflow using LangGraph and integrated it with ASP.NET Core.

Mentor: Is LangGraph the business application?

Student: No, sir. It orchestrates the agent workflow. ASP.NET Core manages business APIs and security.

Mentor: And who decides whether a student is officially eligible for placement?

Student: The authorized business system and responsible human, based on defined placement rules—not the LLM.

Mentor: Excellent!

> A production AI agent is not just an intelligent model. It is an engineered system with state, tools, workflows, security, evaluation and accountability.

## 13. Where we go next

The natural next classroom is:

Lesson 4 – Multi-Agent Systems and MCP in Practice

- Single agent vs multi-agent implementation.
- Agent-to-agent delegation.
- MCP server development using Python.
- Connecting MCP tools to TFLAssessment APIs.
- Agent memory and persistent state.
- Testing multi-agent workflows.
- Security boundaries across agents and tools.

The next practical milestone will be to connect a TFLAssessment MCP server to our LangGraph agent.

🧑‍💻Prepare the next agentic engineering labTap to set up