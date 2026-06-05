Genie Code is  an AI data partner in Databricks that **integrates with Unity Catalog** to understand and operates across your entire data Ecosystem.

- Deep integration with Unity Catalog for context awareness.
- Aligns with governance and standards that we have set up.
- Supports end to end data task
- Free(only pay for Compute)

In Databricks, **Unity Catalog** is the centralized governance and metadata layer for all your data and AI assets.

# Modes:
1. Chat - General Chat Bot (Asks and Explains Stuff)
2. Agent - Ability to Execute 

# Has MCP Capabilities

**MCP (Model Context Protocol)** is an open protocol that allows AI models to securely connect to external tools, data sources, and applications in a standardized way.

In Genie by default we have SharePoint and Google Drive.

![[Pasted image 20260605165512.png|376]]


# Genie Settings
- User
- Workspace
- Skills 
- Workspace Skills
- Serverless Usage Policy 
Priority:
Workspace Inst > User Inst 

Assistant instructions files should only contain up to 20,000 characters
![[Pasted image 20260605123608.png|266]]

### User Instructions - Applicable to the user only
how the agent should behave
### Workspace Instructions - Applicable to the entire workspace
![[Pasted image 20260605160148.png]]

### Agent Skills
what can the agent do

![[Pasted image 20260605162348.png]]


## What are Agent Skills?

Agent Skills are a lightweight, open format for extending AI agent capabilities with specialized knowledge and workflows.At its core, a skill is a folder containing a `SKILL.md` file. This file includes metadata (`name` and `description`, at minimum) and instructions that tell an agent how to perform a specific task. Skills can also bundle scripts, reference materials, templates, and other resources.

```
my-skill/
├── SKILL.md          # Required: metadata + instructions
├── scripts/          # Optional: executable code
├── references/       # Optional: documentation
├── assets/           # Optional: templates, resources
└── ...               # Any additional files or directories
```


### Why Agent Skills?

Agents are increasingly capable, but often don’t have the context they need to do real work reliably. Skills solve this by packaging procedural knowledge and company-, team-, and user-specific context into portable, version-controlled folders that agents load on demand. This gives agents:

- **Domain expertise**: Capture specialized knowledge — from legal review processes to data analysis pipelines to presentation formatting — as reusable instructions and resources.
- **Repeatable workflows**: Turn multi-step tasks into consistent, auditable procedures.
- **Cross-product reuse**: Build a skill once and use it across any skills-compatible agent.


### How do Agent Skills work?

Agents load skills through **progressive disclosure**, in three stages:

1. **Discovery**: At startup, agents load only the name and description of each available skill, just enough to know when it might be relevant.
2. **Activation**: When a task matches a skill’s description, the agent reads the full `SKILL.md` instructions into context.
3. **Execution**: The agent follows the instructions, optionally executing bundled code or loading referenced files as needed.

Full instructions load only when a task calls for them, so agents can keep many skills on hand with only a small context footprint.


