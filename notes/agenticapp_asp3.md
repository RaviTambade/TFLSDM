# Agentic AI Testing and Evaluation


AI Engineering · Quality Engineering

Theme: From Agentic AI Demos to Reliable Agent Systems

## 1. The classroom opening – A new engineering challenge

👨‍🏫 Mentor (Ravi Sir): Om, until now, we have learned how to build AI applications.

We have explored:

- LLM integration.
- RAG and contextual retrieval.
- MCP servers and MCP clients.
- AI agents and tool calling.
- Multi-agent collaboration.
- Agent orchestration using workflows.

Now I want to ask you something. Suppose we build an AI-powered student assessment assistant.

A student asks: "Sir, what skills should I improve to become a full-stack developer?" Our AI agent may perform the following operations:

1. Understand the student's question.
2. Retrieve the student's skill profile.
3. Call the assessment API.
4. Retrieve relevant learning resources.
5. Ask another agent to analyze skill gaps.
6. Generate a personalized learning plan.

Now imagine something goes wrong.

- The agent calls the wrong API.
- The agent retrieves another student's profile.
- The LLM invents assessment scores.
- The MCP tool returns incorrect information.
- The agent repeatedly calls the same tool.
- The final response looks convincing but contains incorrect recommendations.

How do we test such a system?

👨‍🎓 Student (Om): Sir, we can write unit tests and integration tests like traditional applications.

👨‍🏫 Mentor: Absolutely! But there is one important difference. In traditional applications, developers generally define the execution path. In agentic applications, the LLM can influence the execution path by selecting tools, generating arguments and deciding what to do next. Therefore, our testing strategy must evolve. We are no longer testing only code. We are testing decisions, interactions, permissions and outcomes.

## 2. Traditional testing vs Agentic AI testing

| Traditional software        | Agentic AI software                    |
| --------------------------- | -------------------------------------- |
| Predictable execution path  | Dynamic execution paths                |
| Function inputs and outputs | Context, prompts and tool arguments    |
| Deterministic assertions    | Deterministic and semantic assertions  |
| API response validation     | Tool response and grounding validation |
| Business rule validation    | Business rules + AI behavior           |
| Functional correctness      | Functional correctness + safety        |
| Response time               | Latency + tokens + execution cost      |

Consider this comparison:

```
TRADITIONAL SOFTWARE

Input
  |
  v
Function
  |
  v
Business Logic
  |
  v
Output
  |
  v
Assertion
```

```
AGENTIC AI SOFTWARE

User Request
     |
     v
  AI Agent
     |
     v
  LLM Decision
     |
     +------> Tool A
     |
     +------> Tool B
     |
     +------> Tool C
     |
     v
  Observation
     |
     v
  Next Decision
     |
     v
  Final Response
     |
     v
  Evaluation
```

The second architecture requires us to test multiple possible execution paths.

## 3. Agentic AI testing architecture

Let's imagine that we are testing TFLCoMentor.

```
                  QA ENGINEER
                       |
                       v
                 PYTEST FRAMEWORK
                       |
          +------------+-------------+
          |            |             |
          v            v             v
       Unit Tests   Tool Tests   Workflow Tests
          |            |             |
          v            v             v
      Agent Nodes   MCP Tools    Multi-Agent
          |            |          Execution
          |            |             |
          +------------+-------------+
                       |
                       v
                MOCKING LAYER
                       |
             +---------+---------+
             |         |         |
             v         v         v
           Mock LLM  Mock API  Mock MCP
             |         |         |
             +---------+---------+
                       |
                       v
                EVALUATION ENGINE
                       |
          +------------+-------------+
          |            |             |
          v            v             v
       Accuracy    Grounding     Security
          |            |             |
          +------------+-------------+
                       |
                       v
                OBSERVABILITY
                       |
             +---------+----------+
             |         |          |
             v         v          v
          Latency   Tokens       Cost
                       |
                       v
                 TEST REPORT
```

## 4. Eight engineering dimensions

Let's break our classroom into eight practical laboratories.

### Lab 1: Unit testing individual agent nodes

An agent workflow consists of multiple nodes.

For example:

- Intent classification node.
- Student profile retrieval node.
- Skill gap analysis node.
- Learning plan generation node.

We should test each node independently.

Example:

```
def analyze_skill_gap(student_skills, required_skills):

    missing_skills = []

    for skill in required_skills:
        if skill not in student_skills:
            missing_skills.append(skill)

    return missing_skills
```

Test:

```
def test_analyze_skill_gap():

    student_skills = [
        "Python",
        "SQL",
        "Git"
    ]

    required_skills = [
        "Python",
        "SQL",
        "React",
        "Docker"
    ]

    result = analyze_skill_gap(
        student_skills,
        required_skills
    )

    assert result == ["React", "Docker"]
```

Mentor's takeaway: Test deterministic business logic independently of the LLM.

### Lab 2: Testing MCP tools independently

Remember our MCP architecture?

```
AI Agent
   |
   v
MCP Client
   |
   v
MCP Server
   |
   v
Tool
   |
   v
Business API
   |
   v
Database
```

An MCP tool is an interface to a business capability. For example:

```
def get_student_profile(student_id):

    if student_id <= 0:
        raise ValueError("Invalid student ID")

    return {
        "student_id": student_id,
        "name": "Om",
        "skills": ["Python", "SQL"]
    }
```

Test:

```
def test_get_student_profile():

    result = get_student_profile(101)

    assert result["student_id"] == 101
    assert "Python" in result["skills"]
```

Also test:

- Invalid arguments.
- Missing parameters.
- Unauthorized access.
- API failures.
- Timeout conditions.
- Unexpected responses.

### Lab 3: Mocking LLM and business API responses

👨‍🏫 Mentor: Om, should every test call a real LLM?

👨‍🎓 Student: Sir, that could increase execution time and API costs.

👨‍🏫 Mentor: Exactly! We use mocks to isolate our application logic.

```
from unittest.mock import Mock

def test_agent_response():

    llm = Mock()

    llm.invoke.return_value = (
        "The student should improve React."
    )

    response = llm.invoke(
        "Analyze student skills"
    )

    assert "React" in response
```

Mocks help us test:

- Successful LLM responses.
- Empty responses.
- Malformed JSON.
- LLM timeout.
- Rate-limit errors.
- Business API failures.

Important distinction: Mock-based tests verify our application behavior against controlled responses. They do not prove that a real LLM will behave reliably.

### Lab 4: Testing multi-agent workflows

Consider three agents:

```
             USER REQUEST
                  |
                  v
           ORCHESTRATOR AGENT
                  |
          +-------+-------+
          |               |
          v               v
    SKILL AGENT      ASSESSMENT AGENT
          |               |
          v               v
      Skill Gap       Test Results
          |               |
          +-------+-------+
                  |
                  v
          CAREER AGENT
                  |
                  v
          LEARNING PLAN
```

Our tests should verify:

1. Correct agent selection.
2. Correct information transfer.
3. Correct workflow transitions.
4. Handling of agent failures.
5. Termination conditions.
6. Prevention of infinite loops.

For example:

```
def test_workflow_execution():

    result = run_learning_workflow(student_id=101)

    assert result["status"] == "completed"

    assert "learning_plan" in result
```

### Lab 5: Evaluating hallucinations and grounded responses

This is one of the most important areas of AI testing. Suppose the database contains:

```
{
    "student_id": 101,
    "assessment_score": 65
}
```

The AI agent responds: "Your assessment score is 85." Traditional functional testing might not catch this if we only check whether the response is non-empty.

We need grounding evaluation.

| Evaluation   | Question                                           |
| ------------ | -------------------------------------------------- |
| Faithfulness | Is the response supported by retrieved context?    |
| Relevance    | Does it answer the user's question?                |
| Correctness  | Are factual claims accurate?                       |
| Completeness | Does it cover the required information?            |
| Grounding    | Can important claims be traced to trusted sources? |

Example test:

```
def test_grounded_student_score():

    retrieved_context = {
        "assessment_score": 65
    }

    response = generate_response(
        retrieved_context
    )

    assert "65" in response
```

This is only a basic example. Production evaluation should extract factual claims and compare them against authoritative data rather than relying only on string matching.

### Lab 6: Testing unauthorized tool calls

This is where AI engineering meets application security. Imagine a student asks: "Show me another student's assessment results." The agent attempts to call:

```
get_student_assessment(student_id=202)
```

But the authenticated user owns student ID 101. What should happen? The backend must reject the request.

```
def authorize_student_access(logged_in_user_id,requested_student_id):

    if logged_in_user_id != requested_student_id:
        raise PermissionError("Unauthorized student access")

    return True
```

Test:

```
import pytest

def test_unauthorized_student_access():

    with pytest.raises(PermissionError):

        authorize_student_access(101,  202 )
```

Important engineering principle:

Never trust an LLM to enforce authorization. Enforce permissions inside trusted business APIs and MCP tool execution boundaries.

### Lab 7: Measuring latency, token usage and execution cost

AI quality is not just about correctness. An agent might produce an accurate response but take 45 seconds and consume excessive tokens. We need operational measurements.

| Metric               | Purpose                                |
| -------------------- | -------------------------------------- |
| End-to-end latency   | Total response time                    |
| LLM latency          | Time spent waiting for model responses |
| Tool latency         | Time spent executing tools             |
| Input tokens         | Prompt and context consumption         |
| Output tokens        | Generated response consumption         |
| Number of tool calls | Workflow efficiency                    |
| Cost per execution   | Financial efficiency                   |
| Failure rate         | Operational reliability                |

Example:

```
execution_metrics = {
    "latency_ms": 2400,
    "input_tokens": 1200,
    "output_tokens": 450,
    "tool_calls": 3,
    "estimated_cost": 0.025
}
```

These are illustrative values. We can establish performance thresholds:

```
def test_agent_performance(metrics):

    assert metrics["latency_ms"] < 5000

    assert metrics["tool_calls"] <= 5

    assert metrics["input_tokens"] < 5000
```

Cost calculations should use the actual model's pricing and token accounting.

### Lab 8: Building an Agent Evaluation Pipeline using pytest

Now let's bring everything together.

```
                 DEVELOPER
                     |
                     v
                GIT COMMIT
                     |
                     v
                CI PIPELINE
                     |
                     v
                PYTEST RUNNER
                     |
          +----------+----------+
          |          |          |
          v          v          v
        Unit       Tool      Workflow
        Tests      Tests      Tests
          |          |          |
          +----------+----------+
                     |
                     v
               AI EVALUATION
                     |
          +----------+----------+
          |          |          |
          v          v          v
       Grounding  Security   Performance
          |          |          |
          +----------+----------+
                     |
                     v
                TEST REPORT
                     |
              +------+------+
              |             |
              v             v
            PASS           FAIL
              |             |
              v             v
          Deployment     Bug Fixing
```

A possible project structure:

```
agent-testing-lab/
│
├── app/
│   ├── agents/
│   │   ├── orchestrator.py
│   │   ├── skill_agent.py
│   │   └── career_agent.py
│   │
│   ├── tools/
│   │   ├── student_tool.py
│   │   └── assessment_tool.py
│   │
│   └── services/
│       └── llm_service.py
│
├── tests/
│   ├── unit/
│   │   ├── test_skill_agent.py
│   │   └── test_career_agent.py
│   │
│   ├── tools/
│   │   └── test_mcp_tools.py
│   │
│   ├── integration/
│   │   └── test_agent_workflow.py
│   │
│   ├── evaluation/
│   │   ├── test_grounding.py
│   │   └── test_relevance.py
│   │
│   ├── security/
│   │   └── test_authorization.py
│   │
│   └── performance/
│       └── test_metrics.py
│
├── pytest.ini
└── requirements.txt
```

Useful testing technologies:

| Technology         | Purpose                    |
| ------------------ | -------------------------- |
| pytest             | Test framework             |
| unittest.mock      | Mocking dependencies       |
| FastAPI TestClient | API testing                |
| LangSmith          | LLM tracing and evaluation |
| Ragas              | RAG evaluation             |
| DeepEval           | LLM evaluation             |
| OpenTelemetry      | Observability              |

## 5. Classroom assignment: Build an Agent Testing Lab

Hands-on engineering challenge

Problem statement:

Build a Student Career Guidance Agent and develop an automated testing framework for it.

Your agent should:

1. Retrieve a student's profile.
2. Retrieve assessment scores.
3. Identify skill gaps.
4. Recommend learning resources.
5. Generate a learning roadmap.

Student deliverables:

- Implement agent nodes.
- Implement two MCP tools.
- Write pytest unit tests.
- Mock LLM responses.
- Test multi-agent execution.
- Test unauthorized access.
- Evaluate grounded responses.
- Capture latency and token metrics.
- Generate an automated test report.

Suggested acceptance criteria:

| Assessment        | Expected result                  |
| ----------------- | -------------------------------- |
| Unit tests        | All business logic tests pass    |
| MCP tool tests    | Valid and invalid inputs handled |
| Workflow tests    | Expected workflow completes      |
| Security tests    | Unauthorized calls rejected      |
| Grounding tests   | No unsupported assessment facts  |
| Performance tests | Agreed thresholds satisfied      |

## 6. Mentor's closing discussion

👨‍🏫 Ravi Sir: Om, remember one important thing. When you build a traditional application, you test whether the program executes correctly. When you build an AI agent, you must additionally test whether it makes appropriate decisions, selects authorized tools, uses reliable information and produces trustworthy outcomes. There is a fundamental shift in software engineering.

```
Traditional QA
     |
     v
Is the code working?
     |
     v
Is the output correct?


Agentic AI QA
     |
     v
Is the decision appropriate?
     |
     v
Is the tool selection correct?
     |
     v
Is the tool authorized?
     |
     v
Is the retrieved context reliable?
     |
     v
Is the response grounded?
     |
     v
Is the execution efficient?
     |
     v
Can we trust the outcome?
```

One important message for every Transflower student:

AI agents are probabilistic decision-making systems. Their behavior can vary even when the input appears identical.

Therefore, we need more than traditional unit testing. We need deterministic tests for business logic, controlled experiments for agent behavior, evaluation datasets for quality, and continuous monitoring in production.

> A working AI demo proves that an agent can perform a task. A reliable AI engineering system proves that it can perform the task safely, consistently, efficiently and within defined boundaries.

## 7. What comes after this lesson?

Our next engineering progression:

```
Agent Development
       |
       v
Agent Testing
       |
       v
Agent Evaluation
       |
       v
Agent Observability
       |
       v
Agent Security
       |
       v
Agent Deployment
       |
       v
Production Monitoring
       |
       v
Continuous Improvement
```

This is the journey from AI Application Developer to AI Systems Engineer. Don't just build an agent that works. Engineer an agent that deserves to be trusted.