---
layout: default
title: Terminology
---

# Terminology

## Ontology Classes

### Policy Domain Classes

![full_ontology](./statics/ontology.png)

**Policy**
: The central class representing an access control directive.

**Action Class**
: Defines high-level operations (e.g., Read, Write, Delete, Update).

**ActionScope Class**
: Refines the Action into more granular operations relevant to data interaction. For instance, a "Read" Action can encompass "ViewAction Scope" (for direct display of data) and "ProcessActionScope" (for using data in computations or conditions).

**GrantLevel Class**
: Enumerates specific levels of control:
* TableLevel
* ColumnLevel
* RowLevel

**GrantType Class**
: Categorizes the type of access permission:
* Permitted (explicitly allowed)
* Prohibited (explicitly denied)
* Conditional (allowed under specific conditions, which will be defined in reasoning rules).

**PolicyStatus Class**
: Represents the validity or conflict status of a policy in relation to other policies. Instances include `NoConflicts` and `Conflicted`.

### Data Domain Concepts

**Object Class**
: A top-level abstract class representing any data entity.

**Database**
: Represents a single database instance.

**Table**
: Represents a table within a database.

**Column**
: Represents a column within a table.

**Row**
: Represents a specific row within a table.

### Query Domain Concepts

**SQLNode Class**
: The top-level abstract class for any component within an SQL Abstract Syntax Tree (AST).

**Statement Class**
: Represents high-level SQL commands.

**Clause Class**
: Represents specific clauses within SQL statements.

**Expression Class**
: Represents leaf nodes and fundamental components of SQL queries, particularly those referencing data objects or defining operations on them.

**AccessAlignmentStatus Class**
: Defines the status of a SQL node's access in relation to defined access policies. Instances include `Aligned`, `Violated`, and `Conditional`.

## Ontology Relationships

### Intra-Domain Relationships

#### Policy Domain Relationships

**hasAction**
: Connects a Policy instance to an Action instance.

**appliesToScope**
: Connects a Policy instance to an ActionScope instance.

**hasAgent**
: Connects a Policy instance to an Agent instance.

**hasGrantLevel**
: Connects a Policy instance to a GrantLevel instance.

**hasGrantType**
: Connects a Policy instance to a GrantType instance.

**controlAccessTo**
: Connects a Policy instance to one or more Object instances (from the Data domain).

**hasScope**
: Connects an Action instance to an ActionScope instance.

**allowedGrantLevel**
: Connects a GrantType instance to a GrantLevel instance.

#### Data Domain Relationships

**hasTable**
: Connects a Database instance to one or more Table instances.

**hasColumn**
: Connects a Table instance to one or more Column instances.

**columnOfTable**
: The inverse property of `hasColumn`. Connects a Column instance to the Table instance it belongs to.

**hasRow**
: Connects a Table instance to one or more Row instances.

#### Query Domain Relationships

**hasChild**
: Connects an SQLNode to another SQLNode. This property is defined as transitive.

**hasImmediateChild**
: A sub-property of `hasChild`. Connects an SQLNode to its direct child SQLNode in the AST, indicating a non-transitive, direct parent-child relationship.

**hasParent**
: The inverse of `hasChild`. Connects an SQLNode to its parent SQLNode. This property is also transitive.

**hasImmediateParent**
: The inverse of `hasImmediateChild`. Connects an SQLNode to its direct parent SQLNode, indicating a non-transitive, direct child-parent relationship.

**hasNodeType**
: Connects an SQLNode instance to one or more SQLNode Type instances.

**hasAlignmentStatus**
: Connects any Reference type SQLNode (e.g., ColumnReference, TableReference) to an AccessAlignmentStatus instance.

**allowsClause**
: Connects an SQLStatement to a predefined set of permissible Clause types that can appear within it.

**belongsToStatement**
: Connects any SQLNode within a given query to its containing Statement node.

### Inter-Domain Relationships

**representsAction**
: Connects an SQLStatement instance (Query domain) to a unique Action instance (Policy domain).

**executedBy**
: Connects an SQLStatement instance (Query domain) to one and only one Agent instance (Policy domain).

**hasEffectiveScope**
: Connects a Clause instance (Query domain) to an ActionScope instance (Policy domain).

**referencesToColumn**
: Connects a ColumnReference SQLNode instance (Query domain) to exactly one Column instance (Data domain).

**referencesToTable**
: Connects a TableReference SQLNode instance (Query domain) to exactly one Table instance (Data domain).