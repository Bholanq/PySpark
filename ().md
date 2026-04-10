Q1.
An engineer notices a significant increase in the job execution time during the execution of a Spark job. After some investigation, the engineer decides to check the logs produced by the Executors. How should the engineer retrieve the Executor logs to diagnose performance issues in the Spark application? 
A. Use the command 'spark-submit' with the '--verbose' flag to print the logs to the console. 
B. Locate the executor logs on the Spark master node, typically under the /tmp directory. 
C. Use the Spark UI to select the stage and view the executor logs directly from the stages tab. 
D. Fetch the logs by running a Spark job with the `spark-sql` CLI tool

The correct answer is:
**C. Use the Spark UI to select the stage and view the executor logs directly from the stages tab.**

---

### Why C is correct

- The **Spark UI** (usually on port `4040`) provides detailed insights into:
    - Jobs → Stages → Tasks
    - Each **executor’s logs (stdout, stderr)**
- You can:
    1. Go to **Stages tab**
    2. Click a specific stage
    3. Drill down into **tasks**
    4. Click on an executor → view logs
        
This is the **standard and easiest way** to debug performance issues like:
- Skew
- GC pauses
- Memory errors
- Slow task

### Why others are wrong
**A. `--verbose` flag**
- Only prints **driver-side submission details**
- Does NOT give executor logs
    

**B. Logs on master node (/tmp)**
- Incorrect assumption
- Executors run on **worker nodes**, not necessarily master    
- Logs are distributed, not centralized like this

**D. `spark-sql` CLI**
- Used for running SQL queries
- Not for retrieving executor logs    
### Extra Insight (Real-world)
In production setups:
- **YARN** → `yarn logs -applicationId <id>`
- **Databricks** → UI → Executors ta    
- **Kubernetes** → `kubectl logs`

But across all environments, **Spark UI remains the first place to check**.
If you want, I can walk you through **how to interpret executor logs to pinpoint performance bottlenecks** (very useful for interviews).