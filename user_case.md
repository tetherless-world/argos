---
layout: default
title: Use Case 
---

# Use Case: ManagerAgent on Financial Database

## 1. Background

This use case demonstrates how the ARGOS framework applies access control policies for a specific role, the **ManagerAgent**. The agent is designed to interact with a corporate financial database to perform analytical tasks, such as reviewing department budgets, analyzing transaction patterns, and generating summary reports.

The ManagerAgent requires broad access to financial data but must be restricted from viewing sensitive Personally Identifiable Information (PII) of employees and individual salary details to comply with privacy regulations and internal company policies. This scenario highlights how ARGOS can enforce complex, fine-grained access rules that go beyond simple table-level permissions.

## 2. Database Schema

The financial database contains information about employees, departments, transactions, and accounts. The schema is designed to link financial activities to specific employees and departments.

![Financial Database Schema](https://i.imgur.com/wVn4E9U.png)
*(A representative schema for a financial database, showing tables for employees, departments, accounts, and transactions.)*

## 3. Access Control Policies for ManagerAgent

The following table outlines the specific access control policies enforced for the `ManagerAgent`. These policies are defined within the ontology and are used by the reasoner to validate and realign queries.

| Policy ID | Description | Grant Level | Action | Scope | Grant Type |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **P01** | Prohibit viewing of any employee ID or Social Security Number. | Column | Read | View | Prohibited |
| **P02** | Prohibit viewing of individual employee salaries. | Column | Read | View | Prohibited |
| **P03** | Allow processing of employee salaries for aggregation (e.g., SUM, AVG). | Column | Read | Process | Permitted |
| **P04** | Prohibit viewing of any customer's account number. | Column | Read | View | Prohibited |
| **P05** | Prohibit modification of the `Employees` or `Accounts` tables. | Table | Modify | - | Prohibited |
| **P06** | Allow viewing of all transaction details. | Table | Read | View | Permitted |
| **P07** | Allow viewing of department names and budgets. | Table | Read | View | Permitted |
| **P08** | Prohibit access to employee home addresses. | Column | Read | View | Prohibited |

## 4. Ontology ABox (Instance Data)

The Assertion Box (ABox) contains the specific instances (individuals) for this use case, including the `ManagerAgent` instance, the database schema instances (tables, columns), and the policy instances defined above. This file provides the complete set of facts upon which the reasoner operates.

* **[View the ABox for the ManagerAgent Use Case](./abox/manager_agent.ttl)**