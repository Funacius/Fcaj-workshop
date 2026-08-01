---
title: "AgentForge Ho Chi Minh City"
menuTitle: "AgentForge Workshop"
weight: 3
pre: "<b>4.3.</b>"
---

<div class="event-hero-title">Build and Deploy AI Agents on Amazon Bedrock AgentCore with Kiro</div>

## Event Information

**Event name:** AgentForge Ho Chi Minh City  
**Date and time:** August 1, 2026, from 09:00 to 11:00  
**Location:** Bitexco Financial Tower, 2 Hai Trieu Street, Saigon Ward, Ho Chi Minh City 700000, Vietnam  
**Format:** Technical presentation and guided hands-on lab  
**Workshop guide:** [Build and Deploy AI Agents on Amazon Bedrock AgentCore using Vibe Coding with Kiro](http://agentforge-hcmc-workshop-p371s08u.s3-website-ap-southeast-1.amazonaws.com/00-Overview/00-Dashboard-Overview.html)

## Event Objectives

- Understand how Kiro supports natural-language and specification-driven development.
- Learn the responsibilities of Amazon Bedrock AgentCore in operating agents in production.
- Build, test, deploy, and invoke a basic Strands agent using the AgentCore CLI.
- Explore how tools, knowledge sources, authentication, and a Web UI form an end-to-end agentic application.

## Main Content

### Kiro and AI-assisted development

The session introduced Kiro as an AI-native development environment. I explored
its project views, chat panel, integrated terminal, and two complementary ways
of working:

- **Vibe Coding** for focused prototypes, small features, bug fixes, and rapid iteration from natural-language prompts.
- **Spec-Driven Development** for complex or production-oriented work that requires reviewed requirements, acceptance criteria, design, and implementation tasks.
- **Steering documents** stored under `.kiro/steering/` to give Kiro persistent project context and development guidelines.

A central lesson was that generated changes still require human review. Kiro
can accelerate implementation and command execution, but the developer remains
responsible for validating diffs, permissions, dependencies, security, and test
results.

### Amazon Bedrock AgentCore foundations

The workshop explained that creating a model-powered agent is only one part of
the problem. A production system must also address runtime deployment,
auto-scaling, IAM permissions, secure access, tool integration, memory, and
observability. Amazon Bedrock AgentCore provides managed building blocks for
these operational responsibilities.

The reference application was a Returns and Refunds Assistant capable of
looking up customer orders, checking return eligibility, calculating refund
amounts, and retrieving country-specific return policies.

## Hands-on Lab

### Creating and deploying the first agent

I followed the AgentCore CLI workflow to:

1. Scaffold a Python agent project using `agentcore create`.
2. Select Strands Agents SDK with Amazon Bedrock as the model provider.
3. Inspect the generated application and deployment structure.
4. Run the agent locally using `agentcore dev` and test it through the terminal.
5. Deploy it to AgentCore Runtime using `agentcore deploy`.
6. Invoke the deployed cloud agent and verify its response.

This workflow demonstrated how the CLI packages the agent, prepares deployment
resources, and provides a consistent loop from local testing to managed runtime.

### Adding domain behavior and tools

The basic agent was then specialized as a Returns and Refunds Assistant. The
lab covered:

- Defining a focused system prompt for the agent's role and behavior.
- Creating Python tool functions with the Strands `@tool` decorator.
- Using clear docstrings and type hints so the model understands when and how to call a tool.
- Testing order lookup, customer lookup, current time, and return-policy retrieval behavior.
- Reviewing generated code before redeploying the updated agent.

### End-to-end architecture explored

The workshop guide also demonstrated how the agent can evolve beyond inline
mock tools:

- AgentCore Gateway securely routes tool requests.
- AWS Lambda queries customer, order, and product records in Amazon DynamoDB.
- An Amazon Bedrock Knowledge Base supplies country-specific return policies.
- Amazon Cognito provides authentication for gateway calls and the Web UI.
- A Streamlit chat interface allows authenticated users to interact with the deployed agent.
- Amazon CloudWatch provides logs, traces, and operational observability.

## Knowledge Gained

- Vibe Coding is useful for rapid iteration, while Spec-Driven Development is safer for features spanning multiple files or system boundaries.
- Steering documents reduce repeated prompting and keep generated work aligned with project conventions.
- Tools turn an agent from a conversational interface into a system that can perform controlled actions and retrieve application data.
- Tool descriptions, type hints, and return values strongly affect whether an agent invokes a tool correctly.
- Local testing should precede cloud deployment to shorten feedback cycles and reduce unnecessary resource usage.
- Runtime, identity, gateway, and observability are essential parts of a production agent, not optional additions after the model is working.
- AI-assisted development improves speed, but architectural judgment, security review, and verification remain human responsibilities.

## Relevance to EduCloud Lite

The workshop provides a possible direction for future EduCloud research. An
agent could support course discovery, answer questions using approved course
materials, or guide administrators through operational information. Before such
a feature is implemented, EduCloud would need to define permitted tools,
identity boundaries, source-grounded responses, evaluation criteria, logging,
cost limits, and human oversight. The current EduCloud release does not include
AgentCore; this is a future enhancement informed by the workshop.

## Event Evidence

