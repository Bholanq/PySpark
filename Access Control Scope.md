![[Pasted image 20260702072157.png]]

# RBAC
![[Pasted image 20260702072610.png]]

![[Pasted image 20260702072712.png]]

# Service Principals - Creds for a machine

> A **Service Principal** is a non-human identity used by applications, automated jobs, or services to authenticate and access resources securely. Unlike a regular user account, it is intended for machine-to-machine communication and typically authenticates using a client ID and client secret or certificate. In Databricks, Service Principals are commonly used to run Jobs, access Unity Catalog, call APIs, and automate data pipelines without relying on personal user accounts. They are preferred for production workloads because they are more secure, easier to audit, and independent of individual employees.


A Service Principal is required whenever **software—not a person—needs to authenticate and access resources**. Typical examples include:

- Scheduled Databricks Jobs
- ETL/ELT pipelines orchestrated by Airflow or similar tools
- CI/CD deployments
- Applications calling the Databricks REST API
- Azure Data Factory or other orchestration services
- Automated reporting and data synchronization

In all these cases, a Service Principal provides a secure, stable, and auditable identity for machine-to-machine authentication.

# Isolate Business Units

- Isolated env for a single business req

