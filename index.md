---
layout: default
---

# Welcome to the ARGOS Ontology Publication

### Access Realignment using Graph-based Ontological Semantics

The adoption of Large Language Models (LLMs) as natural language interfaces for relational databases (RDBs) is rapidly advancing. However, the practical application of RDB-backed agents is hindered by legacy access control mechanisms that do not fully support the semantic expressiveness and dynamic query generation capabilities of LLMs.

To resolve these shortcomings, we propose **ARGOS**, a novel symbolic access control framework tailored for LLM-RDB integration. The ARGOS framework intercepts LLM queries and uses a symbolic reasoner that employs this novel ontology to accurately infer the access alignment of each data reference based on the agent's assigned data access policies. Instead of merely rejecting a query that violates a data access policy, ARGOS performs automatic query realignment to comply with the agent's data access rights, preserving user intent while guaranteeing access-level security.

![overview_ontology](./statics/extended_ontology_overview.png)
---

### Documentation

Explore the detailed documentation to understand the structure and application of the ARGOS ontology.

* **[Ontology Overview](./ontology.md):** A deep dive into the ontology's structure, including its core domains (Schema, Policy, Query), concepts, and relationships.
* **[Terminology](./terminology.md):** A complete glossary of the classes and properties used within the ontology.
* **[User case](./user_case.md):** A complete user case Financial database back LLM agent.

---

### Contact

For questions, feedback, or inquiries about the ARGOS framework and ontology, please reach out to: <anonymized>