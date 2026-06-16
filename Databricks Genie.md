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
- User Inst
- Workspace Inst
- User Skills 
- Workspace Skills
- Serverless Usage Policy 
Priority:
**Workspace Inst > User Inst** 

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

Agent Skills are a lightweight, open format for extending AI agent capabilities with specialized knowledge and workflows. At its core, a skill is a folder containing a `SKILL.md` file. This file includes metadata (`name` and `description`, at minimum) and instructions that tell an agent how to perform a specific task. Skills can also bundle scripts, reference materials, templates, and other resources.

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

# Guardrails and Limitations

![[Pasted image 20260609154129.png]]

- 20 queries across all genie workspaces
- 20,000 char text instructions limit.
- If the queries in SQL expression/ Query are illogical or create outputs that are too big to handle then it will run its own query.
# Genie Space

These instructions are listed in priority order
## Instructions
1. **Text** - Simple as it sounds just enter the instruction in the text box.
	1. Use this sparringl

2. Joins - 
	**Join instructions** are **metadata annotations** in Genie spaces that help me understand how tables relate to each other. They typically specify:
	
	- **Foreign key relationships** - which columns link tables together (e.g., `customer.c_custkey` joins to `orders.o_custkey`)-
	- **Join types** - whether to use INNER JOIN, LEFT JOIN, etc.
	- **Cardinality** - one-to-many, many-to-one relationships between tables

In **Databricks Genie Spaces**, **join instructions** teach Genie **how different tables are related**, so that when a user asks a question involving multiple datasets, Genie generates the correct SQL `JOIN` statements.

Without join instructions, Genie has to guess how tables should be connected, which often leads to incorrect results, duplicated rows, or failed queries.

```
IF THE JOIN INSTRUCITONS IT SELF IS WRONG, eg. if the join uses one-to-one but the data is clearly one-to-many then genie will use one-to-many join query resolution.
```

3. SQL Expressions
	1. In **Databricks Genie**, a **SQL Expression** is a reusable SQL definition that teaches Genie **how to calculate a business metric or derive a field**. Instead of relying on Genie to infer the logic from natural language, you explicitly provide the SQL that should be used.

### basically when referring to these terms use these expressions 
##### Fields in SQL Expression Instructions

**Fields** define which **columns or calculated expressions** should be included when the SQL expression is used. They specify what data to SELECT.
##### Filter in SQL Expression Instructions
**Filter** defines the **WHERE clause conditions** that should be applied when using this SQL expression. It restricts which rows are included in the calculation.

| Feature               | Purpose                                            |
| --------------------- | -------------------------------------------------- |
| **Join Instructions** | Tell Genie _how tables are related_                |
| **SQL Expressions**   | Tell Genie _how to calculate something_            |
| **Text Instructions** | Tell Genie _how to interpret business terminology_ |

Synonyms field: Enter common ways that users might refer to the expressions colloquially

4. **SQL Quries**
	1. Why use SQL Query Instructions if Genie already generates SQL?
	2. Because Genie may not know your **business-specific logic**.
For example:
Users ask:
> "Show active customers."

But "active customer" actually means:
```
Purchased at least twice in the last 180 days,excluding trial accounts.
```
Genie cannot infer this.
You provide an example query:
```
SELECT customer_idFROM salesWHERE account_type <> 'Trial'GROUP BY customer_idHAVING COUNT(*) >= 2   AND MAX(order_date) >= CURRENT_DATE() - 180;
```
Now Genie understands what **active customer** means.

# Some good practices

![[Pasted image 20260611131203.png|519]]
