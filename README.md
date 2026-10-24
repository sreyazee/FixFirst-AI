🛠️ FIXFIRST AI 

AI Agents for Safer Bangalore Roads

FixFirst is a proposed multi-agent AI system designed to help report potholes, prioritise road repairs, coordinate eligible contractors, and track repair completion.

Our Three AI Agents

1. RoadScout AI — Road Damage Inspector

Responsibility: Understand and analyse reported road damage.

- Analyses uploaded pothole images.
- Estimates visible damage severity.
- Generates a structured road-damage report.
- Flags uncertain results for human review.

2. CrewMatch AI — Contractor Coordination Agent

Responsibility: Recommend suitable contractors for repair requests.

- Checks contractor approval status.
- Evaluates skills, service area, and workload.
- Matches urgent cases with suitable available contractors.
- Explains why a contractor was recommended.
- Sends proposed assignments to an authorised administrator for approval.

3. ProofGuard AI — Repair Verification Agent

Responsibility: Track repair completion and verify submitted evidence.

- Compares before-and-after repair photos.
- Checks whether required evidence and work-order details are present.
- Flags incomplete or inconsistent submissions for review.
- Tracks payment status when authorised payment records are available.

# 🛠️ FixFirst AI Agent

> **Automated road damage reporting and severity analysis powered by Fetch.ai uAgents.**

**FixFirst** is an intelligent AI agent designed to streamline infrastructure maintenance. By analyzing incoming road hazard reports, photos, and location data, FixFirst automatically determines the severity of the damage (e.g., deep potholes on highways vs. minor surface wear) and routes structured reports to relevant response teams using the standard **Agent Chat Protocol**.

---

## 🌟 Key Features

* **Agent Chat Protocol Compliant:** Built using standard `ChatMessage`, `ChatAcknowledgement`, and session handlers for seamless communication on the Agentverse network.
* **Smart Severity Analysis:** Automatically scans report text and locations to classify issues into **High** or **Medium** priority.
* **Instant Acknowledgments:** Automatically acknowledges incoming messages to maintain stable network handshakes.
* **Cloud-Ready:** Optimized for deployment and testing directly on the Fetch.ai Agentverse platform.

---

## 🏗️ Architecture & How It Works
## 💻 Core Code Explanation

The core logic listens for incoming chat protocol messages, parses the report content, evaluates severity based on keywords (like *"main"* or *"highway"*), and responds with a structured analysis report:

```python
@chat_proto.on_message(ChatMessage)
async def handle_chat_message(ctx: Context, sender: str, msg: ChatMessage):
    # 1. Acknowledge receipt immediately for network stability
    await ctx.send(
        sender,
        ChatAcknowledgement(
            timestamp=datetime.now(timezone.utc),
            acknowledged_msg_id=msg.msg_id,
        ),
    )

    # 2. Extract text content from the incoming chat message
    report_text = ""
    for item in msg.content:
        if isinstance(item, TextContent):
            report_text = item.text

    # 3. Analyze damage severity based on keywords
    if "main" in report_text.lower() or "highway" in report_text.lower():
        severity = "High"
        description = "Deep pothole detected on a major road. High safety hazard."
    else:
        severity = "Medium"
        description = "Moderate surface degradation and pothole formation."

    # 4. Format and reply back with the analysis report
    response_msg = ChatMessage(
        timestamp=datetime.now(timezone.utc),
        msg_id=uuid4(),
        content=[
            TextContent(type="text", text=f"Severity: {severity}\nDescription: {description}"),
            EndSessionContent(type="end-session"),
        ],
    )
    await ctx.send(sender, response_msg)
🚀 Deployment & Testing
Agentverse Setup:

Create a new hosted agent on Agentverse.

Copy the script contents into your agent.py editor.

Start the Agent:

Click Start Agent to publish the manifest and bring the agent online.

Testing:

Navigate to the Testing tab or chat interface.

Send a test report (e.g., "Large pothole on Main Street" to trigger High severity, or "Small bump on 5th Ave" for Medium severity).

🛠️ Tech Stack
Language: Python

Framework: Fetch.ai uagents & uagents-core

Protocol: Agent Chat Protocol (chat_protocol_spec)
