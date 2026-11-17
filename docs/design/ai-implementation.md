# AI implementation, model routing, and assistant personalities

Prepared on 15 November 2026 for the f-abdul senior project.

## Decision status

The project owner endorsed the proposed direction of cloud application hosting with paid model APIs, task-based model routing, and three selectable assistant personalities. This document records that direction for team review and implementation planning.

The initial design uses two model tiers, structured task results, and personality-specific response templates. Exact model versions, provider accounts, hosting configuration, routing thresholds, and the three final character identities remain choices to validate. Professional, Companion, and Coach are provisional personality concepts.

The current product direction is calendar-connected reminders and booking in phase 2. The proposed spending and bills features were dropped on 17 November 2026, so the spending examples in the routing tables below are illustrative only. The companion [proposed-features document](../planning/proposed-features.md) contains feature scope, estimates, and validation questions.

## Goals

Provide dependable Thai and English assistance through LINE with low upfront infrastructure investment. Use model capability in proportion to task difficulty and preserve the selected assistant personality across model changes.

Measure correctness, latency, cost per successful task, and perceived personality consistency. Published model prices establish a cost baseline. Model suitability requires evaluation on project examples.

## Hosting options and recommendation

The application backend handles LINE, identity, connectors, records, and scheduled jobs. Model inference interprets requests and writes responses. They scale independently.

| Option | Cost basis | Operational implications | Project fit |
| --- | --- | --- | --- |
| Direct model API | Input and output tokens | Manage quotas, retries, context, and model-version changes | Recommended initial approach |
| Open-weight model through an inference provider | Usually tokens on a shared endpoint | Choose from supported models and verify service and data terms | Benchmark alternative |
| Cloud model service such as Bedrock, Google Cloud, or Microsoft Foundry | Tokens or provisioned capacity | More cloud configuration, region and deployment choices | Useful when cloud controls become important |
| API aggregator such as OpenRouter | Tokens and platform fees | Convenient model access, with an additional service in the data path | Useful for evaluation |
| Serverless GPU on Runpod or Modal | Compute runtime and supporting resources | Deploy the model and handle startup latency and serving configuration | Later custom-model option |
| Managed dedicated endpoint | Instance-hours and replicas | Dedicated capacity with an idle-cost trade-off | Predictable workloads |
| Self-managed rented GPU | GPU-hours, storage, and operations | Own serving deployment, scaling, monitoring, and recovery | Reconsider after measuring traffic |
| Owned hardware | Hardware purchase, electricity, and maintenance | Capacity and uptime depend on owned equipment | Local development and experiments |

Open models can also be accessed through APIs. Review the licence for the exact model before deploying its weights. Together, Bedrock, Microsoft, Hugging Face, Runpod, and Modal document the corresponding deployment and billing options. Sources were consulted on 15 November 2026. [Together pricing](https://www.together.ai/pricing) · [Bedrock pricing](https://aws.amazon.com/bedrock/pricing/) · [Microsoft deployment types](https://learn.microsoft.com/azure/ai-foundry/foundry-models/concepts/deployment-types) · [Hugging Face endpoints](https://huggingface.co/docs/inference-endpoints/pricing) · [Runpod serverless pricing](https://docs.runpod.io/serverless/pricing) · [Modal pricing](https://modal.com/pricing)

The initial implementation recommendation is Cloud Run for the application, managed PostgreSQL for persistent records, a durable queue and scheduler, and paid model API calls. Cloud Run provides usage-based container hosting and automatic scaling. Limit concurrency and instance counts to suit the database and model quotas. Choose the managed database and queue during implementation planning. [Cloud Run pricing](https://cloud.google.com/run/pricing)

Keep provider-specific code behind a small adapter. The application should own task records, conversation state, tool permissions, and personality definitions so a provider change remains manageable.

## Cost baseline

The following standard API prices were retrieved on 15 November 2026. They are a dated research snapshot and must be checked before procurement or deployment.

| Candidate model | USD per million input tokens | USD per million output tokens |
| --- | --- | --- |
| GPT-6 Luna | 0.10 | 0.50 |
| Claude Haiku 5.5, prompts up to 100K tokens | 0.10 | 0.50 |
| Gemini 3.5 Flash-Lite | 0.30 | 2.50 |

These are candidates for evaluation. Availability, features, and pricing can change. [OpenAI pricing](https://developers.openai.com/api/docs/pricing) · [Claude Haiku](https://www.anthropic.com/claude/haiku) · [Gemini pricing](https://ai.google.dev/gemini-api/docs/pricing)

An illustrative workload uses 150 model calls per active user per month, averaging 2,000 input tokens and 300 billable output tokens per call. Input includes instructions, relevant history, and retrieved data.

| Active users | Monthly calls | Luna or Haiku inference, USD | Gemini 3.5 Flash-Lite inference, USD |
| --- | --- | --- | --- |
| 100 | 15,000 | 5.25 | 20.25 |
| 1,000 | 150,000 | 52.50 | 202.50 |
| 10,000 | 1,500,000 | 525.00 | 2,025.00 |

Calculation per call is (input tokens × input price + billable output tokens × output price) / 1,000,000. The table excludes tools, image processing, retries, and application infrastructure. A user message can cause several model calls. Email ingestion adds work independently of chat. Reasoning tokens, when billed, belong in the output accounting.

Track the complete cost of a routed request as routing + task execution + any escalation + response generation + retries + tool charges. Routing adds value only when the measured cost and quality justify the additional work.

Runpod's retrieved example GPU rates were $0.27/hour for RTX A5000 and $0.89/hour for RTX 4090, approximately $197 and $650 over 730 hours. These exclude storage and operations and depend on available hardware. At the illustrative $0.00035 API cost per call, $197 corresponds to approximately 563,000 calls. This is a hardware-only comparison. A migration needs equivalent quality, measured throughput, concurrency, redundancy, and maintenance costs. Serverless workers can incur charges during model loading, execution, and idle waiting before stopping. [Runpod GPU pricing](https://www.runpod.io/pricing) · [Serverless billing](https://docs.runpod.io/serverless/pricing)

LINE messaging has a separate budget. Push messages count toward the plan allowance, while reply messages do not. Track scheduled reminders and summaries separately from interactive replies. [LINE pricing](https://developers.line.biz/en/docs/messaging-api/pricing)

## Application architecture

~~~mermaid
flowchart TD
    L["LINE webhook"] --> I["Verify and deduplicate"]
    I --> Q["Durable job queue"]
    Q --> W["Application worker"]
    W --> R["Task and model routing"]
    R --> M["Selected model API"]
    M --> V["Validate result"]
    V --> T["Authorized tools and records"]
    T --> O["Response with selected personality"]
    O --> N["LINE delivery"]
    S["Scheduled jobs"] --> Q
~~~

Acknowledge the webhook promptly after durable acceptance. Handle duplicate deliveries by event identity and make state-changing operations idempotent. LINE reply tokens are single-use and should be used promptly within one minute of receipt. Define a delayed-response path for longer tasks and include its push-message costs. [LINE webhooks](https://developers.line.biz/en/docs/messaging-api/receiving-messages/) · [LINE API reference](https://developers.line.biz/en/reference/messaging-api/)

Store reminders, transactions, budgets, connector state, and conversation state in the application database. Scheduled reminders run from persisted jobs. Retrieve only the user context needed for each task.

Application code calculates financial totals, checks dates and tool arguments, enforces permissions, and records confirmed outcomes. The model receives actual operation results before reporting success.

## Routing decisions

For each request, decide the task, the necessary model capability, and the response style.

| Decision | Inputs | Output |
| --- | --- | --- |
| Task handling | Intent, available records, missing information, tools | Reminder, spending lookup, summary, plan, or clarification |
| Model selection | Reasoning difficulty, ambiguity, context size, consequences of error | Small model or stronger model |
| Response delivery | Verified result, explanation complexity, selected personality, language | Template, small-model response, or stronger-model response |

Routing is an established workflow. Anthropic describes input classification and routing to specialized tasks and models. RouteLLM demonstrates a cost-quality approach on its evaluated benchmarks. Those results motivate evaluation here and do not establish project-specific savings. [Anthropic routing patterns](https://www.anthropic.com/engineering/building-effective-agents) · [RouteLLM, ICLR 2025](https://proceedings.iclr.cc/paper_files/paper/2025/hash/5503a7c69d48a2f86fc00b3dc09de686-Abstract-Conference.html)

### Proposed initial policy

1. Known buttons and explicit structured actions go directly to their application handlers.
2. Ordinary messages use a small model for task classification and field extraction in one call.
3. Clearly complex tasks route to a stronger model once identified. Avoid an unnecessary full attempt by the small model.
4. Validate required fields, dates, identifiers, allowed operations, and consistency with the user context.
5. Missing user information triggers clarification or an established user preference.
6. Unresolved interpretation or failed semantic checks can trigger a bounded escalation to the stronger model.
7. Execute authorized operations through the application and generate the final response from their actual results.

Start with rules and two model tiers. Learn routing thresholds from measured tasks. Message length alone is a poor complexity measure. Model-reported confidence is an additional signal that requires calibration, rather than a standalone decision rule.

Retried reasoning must reuse recorded task state. Escalation must not repeat an already completed calendar write or reminder creation. Limit model attempts and tool steps per request.

### Task examples

| Request | Task execution | Response path |
| --- | --- | --- |
| Remind me tomorrow to pay my bill | Small-model extraction, date validation, clarification if no time or saved default | Personality-specific template |
| How much did I spend on food this month? | Small-model intent extraction and database calculation | Template or small model |
| Summarize my spending this week | Code computes totals and comparisons, small model summarizes | Small model |
| Help me reduce spending while keeping my gym membership and weekend visits home | Stronger model considers constraints using verified figures | Stronger model |
| Move it to Friday | Resolve the referenced item from conversation and state, clarify if ambiguous | Small model or template |
| Plan my week around deadlines, meetings, and errands | Stronger-model proposal, application checks calendar conflicts | Stronger model for trade-offs, template for saved-event results |

A short reply can follow difficult reasoning. A long source document can require straightforward extraction. Select capability from the work required.

## Response generation

| Situation | Proposed generation method |
| --- | --- |
| Factual confirmation or routine clarification | Application fills a reviewed template for the selected personality |
| Routine explanation of verified data | Small model receives structured facts and personality instructions |
| Nuanced recommendation or complex plan | Stronger model writes the final response with personality instructions included |

A separate writing pass adds latency, tokens, and a chance of semantic drift. Use it where it helps. Supply verified facts, action status, assumptions, uncertainty, and required qualifications to any separate writer. Check that dates, amounts, and completed-action claims survive the transformation.

A task result should carry the intent, relevant facts with source references, operation status, missing fields, and the information the reply must preserve. Its exact schema belongs in implementation planning.

## Three selectable personalities

The user requested a choice of three personalities during onboarding. These concepts were proposed in discussion and remain provisional names and detailed behavior.

| Concept | Behavior | Example clarification |
| --- | --- | --- |
| Professional | Direct, composed, concise, minimal emoji | What time tomorrow should I remind you to pay the bill? |
| Companion | Warm, conversational, lightly playful when appropriate | Sure 😊 What time tomorrow would you like a nudge to pay the bill? |
| Coach | Encouraging, practical, focused on the next step | Let’s get it scheduled. What time tomorrow works for your bill reminder? |

Onboarding presents the same situation in all three styles so users can compare. Store the selected personality and allow switching later. Language and personality should be configurable independently.

Each profile needs a stable identifier and version, a role description, tone, preferred response length, emoji and humour guidelines, preferred form of address, and examples covering success, clarification, errors, corrections, and complex explanations.

For Thai, specify pronouns, polite particles, formality, and Thai/English mixing. Confirm the final character voice with representative users. Adapt expression to the situation, including a straightforward and gentle response when someone is stressed about money.

All personalities use the same verified task facts, permissions, and action status. Differences in warmth or encouragement must preserve amounts, dates, uncertainty, and the meaning of recommendations.

### Consistency across models

Every user-facing model receives the same versioned personality definition, relevant preferences, recent dialogue, and current task state. Backend extraction prompts can focus on structured task accuracy.

Maintain shared state in the application. Retrieve pending tasks and explicit preferences when relevant. Validate conversation summaries so they preserve unresolved questions, corrections, and commitments.

System-role instructions and concrete response examples support consistency. A common prompt improves alignment but does not guarantee identical behavior across models. Evaluate every selected model against the same personality examples. Model-specific prompt adjustments can be recorded under the same personality version. [Output consistency guidance](https://platform.claude.com/docs/en/test-and-evaluate/strengthen-guardrails/increase-consistency)

## Model evaluation and data handling

Benchmark the small-model candidates and at least one stronger model on the same Thai/English tasks. Include Typhoon as a Thai-language candidate. Its official documentation offers hosted and local options. Its free API is described as a research showcase with usage-data collection and no formal support. Use synthetic or redacted examples for that evaluation and assess deployment terms before using personal financial records. [Typhoon deployment options](https://docs.opentyphoon.ai/) · [Typhoon FAQ](https://docs.opentyphoon.ai/en/faq/)

Review training use, retention, and processing location for the exact provider and service. Google distinguishes free- and paid-tier data handling. OpenAI states that API data is excluded from training by default, while default abuse-monitoring logs can retain content for up to 30 days. Minimize personal data passed to the model. [Gemini pricing and tier terms](https://ai.google.dev/gemini-api/docs/pricing) · [OpenAI data controls](https://developers.openai.com/api/docs/guides/your-data)

## Verification approach

Compare routing against both an all-small-model baseline and an all-stronger-model baseline using the same held-out conversations. Include all three personalities and multi-turn conversations that change model tier.

| Dimension | Check |
| --- | --- |
| Task correctness | Correct intent, fields, referenced item, and final operation |
| Clarification | Missing time and ambiguous references produce appropriate questions |
| Factual preservation | Amounts, dates, evidence, and operation status remain correct in the reply |
| Routing | Escalation improves difficult cases and ordinary requests remain economical |
| Personality consistency | Users recognize the intended voice across tasks and tier switches |
| Context continuity | Preferences, previous corrections, and pending questions survive model changes |
| Reliability | Replayed events, retries, and escalation avoid duplicate operations |
| Latency | Measure total response time, including routing and retries |
| Cost | Record all calls, tokens, tool charges, and successful task outcomes |

Include incorrect extraction, failed calendar writes, unavailable connectors, overdue bills, frustrated users, and mixed-language messages. Personality tests should cover these situations as well as successful reminders. Choose numerical thresholds after establishing a baseline, before the main pilot.

## Next implementation decisions

- Select the exact small and stronger models after the benchmark.
- Finalize the three personality identities, Thai voices, and example responses.
- Choose the managed database, queue, scheduler, and deployment region.
- Define the routing and task-result schemas and bounded retry policy.
- Set usage limits, cost monitoring, and latency targets.
- Add model-routing, personality and infrastructure work to the [timeline](../planning/timeline-and-development-phases.md); it is not yet estimated there. (The timeline, advisor briefing and database draft were reconciled to this direction on 17 November; the charter was withdrawn and the SRS stays on its branch.)
- Revisit self-hosting when measured cost, quality, throughput, or deployment requirements justify it.

The earlier 65–110-hour estimate in the feature proposal covers the financial feature increment under its stated assumptions. Additional model-routing, personality, and infrastructure work needs its own estimate during planning.
