# Agentic Engineering Architecture in Practice

Lesson 2: From AI Models to Autonomous Agent Systems

Theme: Don't just connect an LLM to an application. Learn how to engineer a reliable system in which AI can reason, use tools, follow workflows, and operate within controlled boundaries.

## 1. Start with a classroom question

👨‍🏫 Mentor: Om, suppose a student asks an AI chatbot: "Find suitable candidates for a .NET developer position." The AI responds with a list of candidates. Is that an AI agent?

👨‍🎓 Student: Sir, it is using AI to solve a problem. So, probably yes?

👨‍🏫 Mentor: Not necessarily. What if the system can:

- Understand the job description.
- Retrieve candidate profiles.
- Compare skills and project experience.
- Call assessment APIs.
- Calculate a match score.
- Ask a mentor to approve the shortlist.
- Generate a recruitment report.

Now we are moving towards an agentic system. The important difference is not simply that AI generates an answer. It is that the system can take controlled actions toward a goal.

## 2. LLM vs AI assistant vs AI agent

Let's understand these three concepts before discussing architecture.

1. Large Language Model (LLM)

Reasoning and generation engine

- Understands input and generates output.
- Works with tokens and context.
- Does not automatically access business systems.
- Example: Explain a student's Python code.

2. AI Assistant

Interactive helper

- Interacts with users.
- Maintains conversational context.
- May use retrieval and selected tools.
- Example: Answer questions about TAP assessment rules.

3. AI Agent

Goal-oriented system

- Receives a goal.
- Decides which steps are needed.
- Uses permitted tools.
- Observes results and adapts.
- Example: Prepare a candidate shortlist using approved assessment data.

Mentor's takeaway: An LLM generates. An assistant interacts. An agent pursues a goal through controlled actions.

## 3. Anatomy of an AI agent

An agent is more than an LLM with a prompt. Think of it as a small software system with six important parts.

```
             USER / BUSINESS GOAL
                     |
                     v
              +--------------+
              |  AGENT       |
              |  INSTRUCTIONS|
              +--------------+
                     |
                     v
              +--------------+
              | LLM / MODEL  |
              | Reasoning    |
              +--------------+
                     |
             Select next action
                     |
                     v
              +--------------+
              | TOOL REGISTRY|
              +--------------+
                /    |     \
               v     v      v
            Search  API   Database
               \     |      /
                \    |     /
                     v
               TOOL RESULTS
                     |
                     v
               OBSERVATION
                     |
             Continue or finish?
                     |
                     v
                FINAL RESULT
```

| Component    | Responsibility              | Example                            |
| ------------ | --------------------------- | ---------------------------------- |
| Model        | Reasoning and generation    | GPT model                          |
| Instructions | Define role and boundaries  | Candidate evaluation rules         |
| Context      | Supply relevant information | Job description and skill taxonomy |
| Tools        | Perform external operations | Assessment API                     |
| Memory/state | Retain workflow information | Current shortlist                  |
| Guardrails   | Restrict unsafe actions     | No unauthorized candidate data     |

Notice something important: the model does not become the database, API, or authorization system. It requests actions through controlled interfaces.

## 4. Agent runtime and orchestration

👨‍🏫 Mentor: Suppose you have five agents. Who decides which agent works first?

👨‍🎓 Student: The orchestrator, sir?

👨‍🏫 Mentor: Exactly. Think of an orchestra. Musicians may be excellent individually, but someone must coordinate when each one plays.

An agent orchestrator manages:

- Task decomposition.
- Agent selection and delegation.
- Workflow state.
- Tool execution.
- Retries and failure handling.
- Timeouts and budgets.
- Human approval.
- Final result aggregation.

### Orchestration architecture

```
                  USER REQUEST
                       |
                       v
             +---------------------+
             |  API GATEWAY        |
             | JWT / RBAC / Limits |
             +---------------------+
                       |
                       v
             +---------------------+
             | AGENT ORCHESTRATOR  |
             | Goal + State + Plan |
             +---------------------+
                       |
             +---------+---------+
             |         |         |
             v         v         v
         Planning  Retrieval  Evaluation
          Agent     Agent      Agent
             |         |         |
             +---------+---------+
                       |
                       v
              +------------------+
              | TOOL EXECUTION   |
              | APIs / MCP       |
              +------------------+
                       |
                       v
              +------------------+
              | VALIDATION       |
              | Rules + Checks   |
              +------------------+
                       |
                       v
              HUMAN APPROVAL
                       |
                       v
                FINAL RESPONSE
```

The orchestrator is not necessarily another LLM. It can be a deterministic workflow engine, an agent framework, or a combination.

## 5. Single-agent vs multi-agent architecture

A common mistake is assuming that more agents automatically mean a better AI system. They don't. Start with one agent. Introduce multiple agents only when responsibilities genuinely need separation.

Single-agent system

- One agent handles the task.
- Uses multiple tools.
- Easier to debug.
- Less coordination overhead.

Multi-agent system

- Multiple specialized agents.
- Tasks can be delegated.
- Responsibilities can be isolated.
- More complex to monitor.

### Example: TFL CareerFlow

Suppose the business goal is:

"Prepare a shortlist of suitable candidates for a .NET developer role." A single agent might use candidate-search, assessment, and scoring tools in one controlled workflow. A multi-agent design might divide responsibilities:

| Agent            | Responsibility                                |
| ---------------- | --------------------------------------------- |
| Planning Agent   | Understand job requirements and create a plan |
| Profile Agent    | Retrieve authorized candidate profiles        |
| Skill Agent      | Compare .NET, SQL, API and frontend skills    |
| Assessment Agent | Retrieve assessment evidence                  |
| Evaluation Agent | Calculate match scores using approved rules   |
| Reporting Agent  | Prepare shortlist and explain evidence        |

```
                 RECRUITER GOAL
                       |
                       v
               PLANNING AGENT
                       |
          +------------+------------+
          |            |            |
          v            v            v
      PROFILE        SKILL       ASSESSMENT
       AGENT         AGENT         AGENT
          |            |            |
          +------------+------------+
                       |
                       v
                EVALUATION AGENT
                       |
                       v
                REPORTING AGENT
                       |
                       v
                RECRUITER REVIEW
```

Engineering decision: Do not create six agents simply because you can. If a normal service or function can perform the work reliably, use it.

## 6. Choosing an agent orchestration framework

Let's compare three commonly used approaches.

LangGraph

Graph-oriented workflow and state management

- Define nodes, edges and conditional transitions.
- Maintain state across workflow steps.
- Useful for workflows with loops, checkpoints and human approval.

CrewAI

  Role-oriented agent collaboration
- Define agents with roles and goals.
- Assign tasks and coordinate execution.
- Useful for exploring specialized agent teams.

OpenAI Agents SDK

  Agent handoffs, tools and execution tracing
- Define agents and their instructions.
- Give agents tools and handoff capabilities.
- Trace agent execution and tool interactions.

| Requirement                              | Possible starting point           |
| ---------------------------------------- | --------------------------------- |
| Stateful graph with explicit transitions | LangGraph                         |
| Role-based agent collaboration           | CrewAI                            |
| Agent handoffs and tool execution        | OpenAI Agents SDK                 |
| Simple predictable business process      | Conventional code/workflow engine |

These are not interchangeable in every situation. Choose based on workflow complexity, observability, state management, integration needs and team expertise—not popularity.

## 7. MCP: Connecting agents to business capabilities

👨‍🏫 Mentor: Om, imagine our AI agent needs student assessment results. Should we give the model direct database credentials?

👨‍🎓 Student: No, sir. It should call an authorized API.

👨‍🏫 Mentor: Correct. And MCP can provide a standardized way for AI applications to discover and invoke tools exposed by external systems.

Model Context Protocol (MCP) is a protocol for connecting AI applications to external tools and context providers.

### MCP architecture

```
          AI APPLICATION
                |
                v
           LLM / AGENT
                |
                v
            MCP CLIENT
                |
          MCP PROTOCOL
                |
                v
            MCP SERVER
                |
       +--------+---------+
       |        |         |
       v        v         v
   Student  Assessment  Skill
    API        API       API
       |        |         |
       +--------+---------+
                |
                v
          BUSINESS SYSTEMS
```

For TFLCoMentor, an MCP server might expose tools such as:

```
get_student_profile(student_id)
get_assessment_result(student_id, assessment_id)
get_skill_profile(student_id)
get_learning_recommendations(student_id)
```

The agent can request these tools, but the business backend must still enforce authentication, authorization, validation and business rules.

Remember: MCP connects systems. It does not replace security or business logic.

## 8. Human-in-the-loop: Where should humans intervene?

Not every decision should be autonomous. Consider a student performance recommendation system.

| Action                                   | Automation level                                 |
| ---------------------------------------- | ------------------------------------------------ |
| Retrieve course notes                    | Automatic                                        |
| Summarize assessment results             | Automatic with validation                        |
| Recommend learning resources             | Automatic, subject to review                     |
| Change a student's placement eligibility | Human approval                                   |
| Publish an official result               | Authorized human or controlled business workflow |

### Human approval workflow

```
          AGENT PROPOSES ACTION
                   |
                   v
           POLICY VALIDATION
                   |
                   v
           APPROVAL REQUIRED?
              /         \
            NO           YES
            |             |
            v             v
        EXECUTE       MENTOR REVIEW
            |             |
            |        +----+----+
            |        |         |
            |      APPROVE   REJECT
            |        |         |
            |        v         v
            |     EXECUTE    STOP
            |        |
            +--------+
                 |
                 v
              AUDIT LOG
```

A useful engineering principle:

> The more consequential the action, the stronger the approval and verification requirements should be.

## 9. Agent evaluation, tracing and guardrails

👨‍🏫 Mentor: If an agent gives a wrong recommendation, how will you know which step failed?

👨‍🎓 Student: We need logs, sir.

👨‍🏫 Mentor: Logs are a start. But an agent system needs more than ordinary application logging.

It needs to capture the reasoning workflow's observable events, tool interactions, decisions and outcomes.

### Observability architecture

```
                 USER REQUEST
                      |
                      v
                AGENT SYSTEM
                      |
         +------------+------------+
         |            |            |
         v            v            v
      MODEL         TOOLS        WORKFLOW
      CALLS         CALLS         EVENTS
         |            |            |
         +------------+------------+
                      |
                      v
                TRACE / SPANS
                      |
          +-----------+-----------+
          |           |           |
          v           v           v
       LATENCY      ERRORS      COST / TOKENS
          |           |           |
          +-----------+-----------+
                      |
                      v
                 EVALUATION
                      |
                      v
               IMPROVEMENT LOOP
```

Useful tools in this area include:

- LangSmith – tracing and evaluation workflows.
- Langfuse – traces, prompts and metrics.
- OpenTelemetry – traces, metrics and logs.
- Ragas – evaluating retrieval-augmented generation.
- DeepEval – evaluating model and agent outputs.

### Five important evaluation dimensions

| Dimension         | Question                                         |
| ----------------- | ------------------------------------------------ |
| Correctness       | Is the answer factually correct?                 |
| Grounding         | Is it supported by trusted data?                 |
| Tool accuracy     | Did it call the right tool with valid arguments? |
| Policy compliance | Did it respect access and business rules?        |
| Reliability       | Does it complete consistently within limits?     |

For example, an agent that recommends a student for placement must not invent assessment scores or treat an incomplete assessment as a completed one. A successful agent is not one that merely produces a convincing response. It is one that produces a verifiable and policy-compliant outcome.

## 10. Practical Transflower lab: Build a Student Assessment Agent

Hands-on engineering exercise

Business problem

A mentor wants to understand a student's performance and receive an evidence-based learning improvement plan.

The student asks: "Analyze my assessment performance and tell me which skills I should improve."

### Proposed technology stack

| Layer               | Technology                       |
| ------------------- | -------------------------------- |
| Frontend            | React                            |
| Backend             | ASP.NET Core Web API             |
| Database            | MySQL                            |
| AI model            | LLM                              |
| Orchestration       | LangGraph or OpenAI Agents SDK   |
| Knowledge retrieval | RAG + vector database            |
| Tool integration    | MCP / REST APIs                  |
| Authentication      | JWT + RBAC                       |
| Observability       | OpenTelemetry + tracing platform |

### System architecture

```
              STUDENT / MENTOR
                      |
                      v
              REACT CHAT INTERFACE
                      |
                      v
              ASP.NET CORE API
               JWT / RBAC / ACL
                      |
                      v
              AI ORCHESTRATION
                      |
          +-----------+-----------+
          |           |           |
          v           v           v
       PROFILE     ASSESSMENT    KNOWLEDGE
        TOOL         TOOL         TOOL
          |           |           |
          v           v           v
       STUDENT     RESULTS       RAG
         API         API       CONTENT
          |           |           |
          +-----------+-----------+
                      |
                      v
                 LLM / AGENT
                      |
                      v
               VALIDATION LAYER
                      |
                      v
                MENTOR REVIEW
                      |
                      v
              LEARNING PLAN OUTPUT
```

### Agent workflow

```
START
  |
  v
Understand student request
  |
  v
Authenticate and authorize
  |
  v
Retrieve student profile
  |
  v
Retrieve assessment results
  |
  v
Retrieve skill taxonomy and rubrics
  |
  v
Analyze skill gaps
  |
  v
Generate improvement plan
  |
  v
Validate against evidence
  |
  v
Present recommendation
  |
  v
Mentor feedback
  |
  v
END
```

### Example tool contracts

The following are illustrative API contracts. The backend must verify that the authenticated student can access the requested records.

```
GET /api/students/{studentId}/profile

GET /api/students/{studentId}/assessments

GET /api/skills/{skillId}

GET /api/students/{studentId}/learning-history
```

The agent should receive structured data rather than unrestricted database access.

### Example output

Student skill analysis

Python fundamentals

# 82%

SQL queries

# 65%

API testing

# 45%

Suggested improvement plan

1. Practice SQL joins and aggregation.
2. Write API test cases using pytest.
3. Automate API validation using HTTPX.
4. Demonstrate the work through a mini project.

Illustrative data only—not actual student results.

## 11. The agent engineering lifecycle

We can now connect all the concepts into one lifecycle.

```
       1. PROBLEM UNDERSTANDING
                 |
                 v
       2. REQUIREMENT DECOMPOSITION
                 |
                 v
       3. SYSTEM ARCHITECTURE
                 |
                 v
       4. CONTEXT ENGINEERING
                 |
                 v
       5. AGENT / TOOL DESIGN
                 |
                 v
       6. ORCHESTRATION
                 |
                 v
       7. SECURITY & GUARDRAILS
                 |
                 v
       8. TESTING & EVALUATION
                 |
                 v
       9. DEPLOYMENT & OBSERVABILITY
                 |
                 v
      10. FEEDBACK & IMPROVEMENT
                 |
                 +------> Repeat
```

This is an engineering lifecycle, not simply a prompt-writing lifecycle.

## 12. Student assignment

Build a mini Student Assessment Agent.

Each team should demonstrate:

1. A React chat interface.
2. An authenticated ASP.NET Core backend.
3. One LLM-powered agent.
4. At least three business tools.
5. Retrieval from a trusted learning knowledge base.
6. Structured learning recommendations.
7. Human approval for consequential actions.
8. Tracing and evaluation of agent execution.

### Assessment rubric

| Assessment area                  | Marks |
| -------------------------------- | ----- |
| Problem understanding and design | 15    |
| Backend and API integration      | 15    |
| Agent and tool implementation    | 20    |
| Context and RAG                  | 15    |
| Security and guardrails          | 15    |
| Testing and evaluation           | 10    |
| Demonstration and documentation  | 10    |
| Total                            | 100   |

## 13. Classroom discussion: Think like an engineer

👨‍🏫 Mentor: Om, if an agent can write code, why do we need software engineers?

👨‍🎓 Student: Sir, because someone must understand the requirements, design the architecture and verify the output.

👨‍🏫 Mentor: Excellent. And if an agent can call APIs, should it be allowed to call every API?

👨‍🎓 Student: No, sir. It should only access authorized tools and permitted operations.

👨‍🏫 Mentor: Exactly. That is the difference between experimenting with AI and engineering an AI-powered enterprise system.

## 14. What should students remember?

| Earlier software engineering | Agentic engineering                  |
| ---------------------------- | ------------------------------------ |
| Write functions              | Design agent capabilities            |
| Call APIs                    | Design controlled tool access        |
| Manage application state     | Manage agent workflow state          |
| Write test cases             | Evaluate model and workflow behavior |
| Debug code                   | Trace decisions and tool execution   |
| Enforce business rules       | Enforce rules around agent actions   |
| Deploy applications          | Operate and monitor agent systems    |

Ravi Sir's closing message:

> Don't become an operator of AI tools. Become an engineer who can design, build, test, secure and operate AI-powered systems.
>
> The LLM provides intelligence. Tools provide capabilities. Orchestration provides coordination. Governance provides control. Engineering provides trust.

Next classroom topic: Building a Production-Ready AI Agent with LangGraph and ASP.NET Core — from workflow design to implementation, tool calling, persistence, and testing.&#x20;