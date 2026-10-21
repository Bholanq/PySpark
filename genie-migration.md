# genie-migration

> **Prerequisite:** The export and deployment scripts are intended to be run from **Databricks notebooks**. No local Python environment or `requirements.txt` is required.

GitHub repository for managing and reviewing **Databricks Genie Space** changes before promoting them from DEV to PROD.

> **Note:** PROD deployment is performed manually. This repository does not automatically deploy changes to PROD.


## Workflow

```text
DEV
 │
 │ Develop & test Genie Space
 ▼
Feature Branch
 │
 │ Push configuration
 ▼
Pull Request
 │
 │ Reviewer approval
 ▼
main
 │
 │ Manual migration
 ▼
PROD
```

## Process

1. **Develop & test** the Genie Space in the DEV Databricks workspace.
    
2. **Export/update** the Genie Space configuration in this repository.
    
3. **Create a feature branch** and push the changes.
    
4. **Open a Pull Request (PR)** from the feature branch to `main`.
    
5. **Reviewer reviews and approves** the changes.
    
6. **Merge the PR into `main`** once approved.
    
7. **Manually pull the approved configuration** from `main` and update the PROD Genie Space.
    

## Branching

- `main` — Approved production configuration
    
- `feature/*` — Changes under development/review
    

Direct pushes to `main` should be restricted through **GitHub branch protection**.


## Environments

|Environment|Catalog|
|---|---|
|DEV|`com_edp_dev`|
|PROD|`com_edp_prd`|

# Configuration Variables

When migrating a different Genie Space, the following variables must be reviewed.

## Genie Space JSON downloader script

### 1. DEV Genie Space ID

Find the Genie Space ID in the **DEV Databricks Genie UI**.

Update:

```python
space = w.genie.get_space(
    space_id="<DEV_GENIE_SPACE_ID>"
)
```

Example:

```python
space = w.genie.get_space(
    space_id="01f17b92c67816e6b8c66cbf896f5990"
)
```

This is the Genie Space that will be exported from DEV.


### 2. DEV → PROD Catalog Mapping

The script converts DEV catalog references to PROD catalog references.

Current mapping:

```python
serialized_text = serialized_text.replace(
    "com_edp_dev",
    "com_edp_prd"
)
```

If the DEV or PROD catalog changes, update these values.

| Environment | Catalog       |
| ----------- | ------------- |
| DEV         | `com_edp_dev` |
| PROD        | `com_edp_prd` |

### 3. Tables with Different DEV/PROD Paths

If a table has a different schema or path between DEV and PROD, add a specific mapping.

Current example:

```python
if t["identifier"] == "com_edp_prd.com_consm.hcp_hco_affiliation":
    t["identifier"] = "com_edp_prd.cmpa_insights_internal_schema.hcp_hco_affiliation"
```

This mapping is required because `hcp_hco_affiliation` has a different schema path in PROD.

For another table with a different path, add another mapping following the same pattern.

### 4. DEV Table Used for Column Comments

The script reads Unity Catalog column comments from the DEV tables:

```python
dev_identifier = prd_identifier.replace(
    "com_edp_prd",
    "com_edp_dev"
)
```

If a table has a different DEV path, add the corresponding exception mapping.

### 5. Export Output Path

Update the output file name for the Genie Space:

```python
output_path = "/Workspace/Users/<USER>/GENIE_CICD/Spaces/<SPACE_NAME>.json"
```

Example:

```python
output_path = "/Workspace/Users/<USER>/GENIE_CICD/Spaces/PatientJourney&ClaimsAnalytics.json"
```

The file name should correspond to the Genie Space being migrated.

# Create Genie Space from JSON Script

Before deploying a Genie Space to PROD, review the following variables.

## 1. PROD Warehouse ID

Update:

```python
PRD_WAREHOUSE_ID = "<PROD_WAREHOUSE_ID>"
```

Example:

```python
PRD_WAREHOUSE_ID = "f7db25971d8427e2"
```

This should be the SQL Warehouse that the PROD Genie Space will use.

## 2. PROD Space ID

Update:

```python
PRD_SPACE_ID = "<PROD_SPACE_ID>"
```

If the PROD Genie Space already exists, provide its `space_id`:

```python
PRD_SPACE_ID = "01f1a0873b1c1f16a5a36947b871e143"
```

The script will update the existing Genie Space.

### Creating a New PROD Genie Space

If the Genie Space does not exist in PROD, set:

```python
PRD_SPACE_ID = None
```

The script will then create a new Genie Space.

## 3. PROD Parent Path

Update this if the Genie Space should be created under a different workspace folder:

```python
PARENT_PATH = "/Workspace/Users/<USER>"
```

Example:

```python
PARENT_PATH = "/Workspace/Users/<USER>"
```

## 4. Input JSON File

Update the path to the approved JSON file:

```python
with open(
    "/Workspace/Users/<USER>/GENIE_CICD/Spaces/<SPACE_NAME>.json",
    "r"
) as f:
    _exported = json.load(f)
```

Example:

```python
with open(
    "/Workspace/Users/<USER>/GENIE_CICD/Spaces/PatientJourney&ClaimsAnalytics.json",
    "r"
) as f:
    _exported = json.load(f)
```

The JSON should be the version that was **approved and merged into `main`**.
