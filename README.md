# ARGOS
Securing the Information Access for AI Agents on Enterprise Relational Data

## Abstract 
> Traditional access control systems assume a deterministic, finite set of predefined data operations (actions), tightly coupling access rights to these actions. Retrieval-Augmented Generation (RAG) systems, however, connect large language models (LLMs) to enterprise databases, enabling dynamically generated, context-specific actions for which defining individual policies is both impractical and conceptually flawed. We introduce the ARGOS (Action Realignment using Graph-based Ontological Semantics), an ontology design pattern, which semantically represents actions, aligns them with structured data schemas, and governs access based on user-granted information operation boundaries. ARGOS employs a formal semantic policy framework with an extensible rule set to detect violations at the granularity of individual data operations and provide fine-grained, actionable feedback, thus moving beyond the conventional binary grant/reject paradigm toward context-aware, explainable enforcement.

## Ontology
![ontology](/statics/ontology_figure.png)
