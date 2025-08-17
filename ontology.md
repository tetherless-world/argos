---
layout: default
title: Ontology
---

# Ontology

## High-level Introduction 

Inspired by Ontology-Based Access Control (OBAC) systems, this ontology provides a unified semantic representation across three distinct but interconnected domains: the database schema, access control policies, and the SQL Abstract Syntax Tree (AST). The structure is designed to transform a standard SQL query into an Abstract Semantic Graph (ASG), a rich, graph-based representation that enables formal, automated reasoning on the query's semantic properties and its compliance with security policies.

This ontology is organized into three conceptual "worlds" that work in concert:
- Schema World: A conceptual model of the relational database structure.
- Policy World: A formal definition of the access control rules.
- AST/Query World: A semantic representation of an incoming SQL query.

### Ontology Files 
- [Ontology](./view_tbox.html)
- [Finance Instance](./abox.html)

## Ontology View 
The following diagram provides a high-level visualization of the ontology's structure, showing the key concepts within each world and the relationships that connect them.

![ontology](./statics/extended_ontology_overview.png)

### Schema World 
Our ontological model is designed to represent the database schema for the purpose of governing access, rather than modeling the data instances contained within it. The model is composed of three primary classes—``Database`, `Table`, and `Column`—which allow for the decomposition of the relational schema into a graph of interconnected individuals. Notably, the model deliberately omits a `Row` class. This is because rows in a relational database are not accessed as distinct entities but are instead identified through value-based conditions applied to columns, such as in a `WHERE` clause (e.g., `user\_id = 'user\_1'`). To accurately represent relational integrity, foreign key columns are modeled as single `Column` individuals that link their respective `Table` individuals through dedicated object properties such as `primaryKey` or `foreignKey`. Conversely, columns that share the same name across different tables but are not linked by a foreign key relationship are modeled as distinct individuals to prevent semantic ambiguity. These design decisions are foundational to our approach, providing a precise yet manageable representation of the schema that directly supports our primary goal of defining and enforcing fine-grained semantic access policies.

### Policy World 
Our policy protocol moves beyond traditional object-level control (on tables, columns, and rows) to enable control over the `semantic access} to those objects. This means we consider not only the high-level operation being performed (`hasAction`) but also the specific data usage within different scopes of that action. To achieve this, our protocol, inspired by frameworks like XACML, provides granular control at multiple levels: by object (`GrantLevel`), by operation (`hasAction`), and, most critically, by sub-operational scope (`hasActionScope`). The cornerstone of this approach is the `ActionScope` class, which allows us to control user actions down to an operational scope granularity. For instance, for `Read` actions, we define two crucial scopes: `ViewActionScope` and `ProcessActionScope`. This allows us to distinguish between two fundamental operations within a single SQL query: the __view__ operation (e.g., in a `SELECT` clause), which directly exposes data, and the __process__ operation (e.g., in a `WHERE` clause), which uses data in background computations. This operational distinction, combined with other attributes like `GrantType` (`Permitted`/`Prohibited`) and `Condition` for dynamic `RowLevel` control, forms a highly expressive policy system. While administrators can define both `Permitted` and `Prohibited` policies for ease of use, the system automatically converts `Permitted` grants into an equivalent set of `Prohibited` policies, as the reasoner operates exclusively on prohibitions. The four main attributes in our policy definition protocol, through their various configurations, are sufficient to cover all possible database access scenarios corresponding to the four primary SQL query types.

### AST/SQL World
The purpose of this domain is to lift a syntactic SQL AST into a semantic, graph-based representation upon which the reasoner can operate. Here we model a customized version of the query's AST, focusing only on components relevant to access control rather than a more complete, verbose taxonomy. The transformation begins by asserting high-level semantic attributes, such as mapping a `SelectStatement` to a `ReadAction` via the `representsAction` predicate and tagging the query with the executing `Agent` using the `executedBy` predicate. Next, the ontology models the operational scope of each `Clause` using the `representsActionScope` predicate. This scope is then propagated down to individual `Reference` nodes as an `effectiveActionScope`, which precisely identifies the context in which data is being used. The most critical function of this domain, however, is to map the query's `ColumnRef} and `TableRef` nodes to their corresponding `Column` and `Table` entities in the Schema Domain. This explicit mapping between the dynamic query and the static schema creates the essential bridge across the three domains, enabling the reasoner to perform its validation.

## Usage of the Ontology
The ontology is the core component of a real-time framework that secures LLM-generated queries. The process is as follows:

1. Query Transformation: An incoming SQL query is parsed into a standard AST.
2. Semantic Lifting: This AST is then "lifted" into an instance of the ontology, creating the Abstract Semantic Graph (ASG). This step involves creating ColumnRef and TableRef instances in the Query World and linking them to their corresponding Column and Table instances in the Schema World.
3. Reasoning: The ASG, which now semantically represents the query and its relationship to the database schema and policies, is passed to the reasoner for validation.

## The Reasoner
The symbolic reasoner is the engine that enforces the access control logic defined in the ontology. Its primary function is to operate on the Abstract Semantic Graph (ASG) and perform logical inference.

By analyzing the connections between the Query, Schema, and Policy worlds within the ASG, the reasoner can:

- Assert Semantic Attributes: Automatically infer the semantic intent of different parts of the query (e.g., identifying that a SELECT clause has a ViewActionScope).

- Detect Violations: Formally check if any data reference in the query violates a policy assigned to the agent. For example, it can determine if a ColumnRef points to a Column that is prohibited by an active Policy.

- Guide Realignment: Provide the necessary information to a realignment module, which can then prune the violating parts of the query's AST before it is executed, ensuring security without outright rejecting the user's request.