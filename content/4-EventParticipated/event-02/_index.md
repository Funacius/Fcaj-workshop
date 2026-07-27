---
title: "FCAJ x Agentic AI Build Week 2026"
menuTitle: "AABW Awards & Showcase"
weight: 2
pre: "<b>4.2.</b>"
---

# Event 2

<div class="event-hero-title">Summary Report: “FCAJ x Agentic AI Build Week 2026 — Hackathon Awards & Agentic AI Project Showcase”</div>

## Event Information

**Event Name:** FCAJ x Agentic AI Build Week 2026 — Hackathon Awards & Project Showcase

**Date & Time:** July 25, 2026

**Location:** AWS Office, 26th Floor, Bitexco Tower, 02 Hai Trieu Street, Saigon Ward, Ho Chi Minh City

**Organizers:** First Cloud AI Journey (FCAJ), Amazon Web Services (AWS), and the Agentic AI Build Week community

## Event Objectives

- Recognize the teams and solutions highlighted at the Agentic AI Build Week (AABW) Hackathon.
- Share real hackathon journeys, including rapid prototyping, architecture decisions, failures, iteration, and final demonstrations.
- Present practical Agentic AI products built for customer ordering, cloud architecture design, strategic intelligence, and crowd safety.
- Help community members understand how AWS services can support AI agents from prototype to production.

## Presenters and Project Teams

- **3KA — S.H.E.P.H.E.R.D:** Huynh An Khuong, Nguyen Quoc Huy, Ngo Quang Khoi, Hoang Le Thanh Duc, Dang Nguyen Phuoc Loc, and Dang Truong Hung.
- **OneTeam — KFC Bot Agent:** Anh Duy, Tran Dong, Doan Trung, Minh Viet, and Anshul Roy.
- **Plan V — Solution Architect Professional Native App:** Pham Tien Thuan Phat, Huynh Hoang Long, Le Minh Nghia, Tran Dai Vi, and Nguyen An.
- **SignalScout:** Le Tan Luc, Do Hoang Hieu, Trieu Quoc Hao, Nguyen Van Duy Khiem, Nguyen Cong Minh, and Nguyen Tran Minh Quan.

## Key Highlights

### The 3KA hackathon journey and S.H.E.P.H.E.R.D

- Team 3KA described the complete journey from selecting a track and organizing roles to building, failing, refining the scope, and presenting a working demo under time pressure.
- Their project, **S.H.E.P.H.E.R.D**, evaluates human flow, predicts congestion, detects hazards, and supports response and dispatch decisions.
- The prototype combines **YOLO and ByteTrack** for crowd detection and tracking, **Amazon SageMaker** for model workloads, **Amazon Bedrock AgentCore and Strands Agents** for agent behavior, and a React operations dashboard.
- The team emphasized that a small, complete, explainable demo is more valuable than an oversized idea that cannot be finished reliably.

### OneTeam and the award-winning KFC Bot Agent

- OneTeam presented a multi-channel conversational ordering agent that allows customers to place orders through familiar messaging channels such as Zalo and WhatsApp.
- The solution follows an agent loop of **Goal → Plan → Tools → Act → Verify**, with channel adapters separated from reusable ordering tools and business logic.
- The presentation reported approximately **USD 0.006 per order**, an estimated **USD 88 monthly operating cost**, and a response latency of roughly **three to five seconds**.
- The project demonstrated how Amazon Bedrock AgentCore can reduce infrastructure code while keeping the agent modular and extensible across channels.

### Plan V and the Solution Architect Professional Native App

- Plan V addressed the time-consuming work of extracting requirements, drafting cloud architecture, preparing diagrams, and estimating costs.
- The application converts natural-language requirements into an architecture draft aligned with standards, generates editable draw.io diagrams using official AWS icons, and produces a directional cost estimate for the `ap-southeast-1` Region.
- It also exposes assumptions and missing requirements so that a solution architect can review and refine the result through conversation instead of accepting an opaque answer.

### SignalScout for evidence-backed strategic decisions

- SignalScout is an AI decision-support platform that detects organizational and market signals, analyzes business scenarios, and recommends whether to maintain, adapt, or accelerate a strategy.
- The architecture uses AWS services including Amazon Bedrock, AgentCore, Amazon Cognito, AWS Lambda, Amazon API Gateway, Amazon DynamoDB, Amazon S3, AWS WAF, AWS CloudTrail, Amazon CloudWatch, and AWS Secrets Manager.
- The team included realistic cost scenarios and explained the trade-offs between data collection, model usage, observability, security, and operational scale.

## Key Takeaways

### Technical knowledge

- An AI agent needs a clear goal, an explicit plan, reliable tools, verification, and observable results; a model response alone is not a production system.
- Modular adapters and tool interfaces make it easier to reuse one agent across channels and replace external integrations without redesigning the whole application.
- Architecture diagrams, cost estimates, monitoring, security controls, and human review should be considered from the beginning rather than added after the demo.
- Visual AI systems depend heavily on data quality, camera placement, tracking stability, inference latency, and fallback behavior.

### Practical value

- A successful hackathon team limits scope early, assigns roles clearly, prepares the demo path, and prioritizes one working end-to-end scenario.
- Product value must be communicated with measurable outcomes such as latency, cost per transaction, time saved, reliability, and the quality of decisions.
- Awards and rankings are valuable, but the most reusable outcome is the engineering process: testing assumptions, responding to failures, and explaining why each service is used.

### Connection to EduCloud Lite

- The presentations reinforced the value of separating the frontend, backend, identity, storage, and database responsibilities in EduCloud Lite.
- Plan V's approach to editable AWS architecture diagrams and explicit assumptions is useful for documenting EduCloud's deployment and helping reviewers understand its request flow.
- OneTeam's focus on modular integrations supports the way EduCloud separates authentication, course APIs, file delivery, and the user interface.
- SignalScout's cost scenarios encouraged me to keep the EduCloud deployment small, monitor the services actually used, and avoid adding unnecessary infrastructure.

## Applying to Work

- Define a narrow, demonstrable workflow before adding optional features.
- Document architecture, responsibilities, data flow, security boundaries, and operating cost alongside the source code.
- Treat external services as replaceable integrations and keep credentials outside the repository.
- Test the full user journey and prepare a reliable demonstration instead of validating each component only in isolation.
- Use human review for important decisions and ensure automated outputs remain explainable.

## Event Experience

This event helped me see how different teams transformed Agentic AI ideas into products that could be demonstrated, measured, and discussed in business terms. The award and project-sharing sessions were especially useful because they covered not only successful results but also failed attempts, constraints, and design trade-offs.

### Learning from the teams

- I learned how teams explain a complex architecture by starting with the user problem and then mapping each AWS service to a specific responsibility.
- Comparing four projects showed that the same Agentic AI principles can support very different domains: retail ordering, solution architecture, strategic analysis, and physical-space safety.

### Community and discussion experience

- The presentations provided concrete examples of scope management, teamwork, technical storytelling, and demo preparation under a short deadline.
- Seeing both winning solutions and other strong finalists made the evaluation criteria clearer: useful outcomes, working execution, explainable architecture, and production awareness.

### Lessons learned

- A focused implementation that works end to end is stronger than a broad design with unfinished critical paths.
- Cost, latency, observability, safety, and user trust are product requirements, not only infrastructure concerns.
- Agentic AI should support people with grounded evidence and controllable actions rather than remove human accountability.

## Event Evidence

![Evidence photo from the FCAJ x Agentic AI Build Week 2026 awards and project showcase](/images/events/event-02.jpg)

> Overall, FCAJ x Agentic AI Build Week 2026 gave me a practical view of how teams build, evaluate, present, and improve Agentic AI solutions on AWS. It also provided concrete lessons that I can apply to EduCloud Lite in architecture documentation, deployment planning, cost control, and end-to-end testing.
