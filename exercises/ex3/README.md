# Exercise 3 – AI-Enabled Operations with SAP Joule and SAP Automation Pilot

In the previous exercises, you created and extended operational automations for SAP HANA Cloud using SAP Automation Pilot.

While these automations can already be executed manually, scheduled periodically, or triggered automatically by events, modern operations teams increasingly rely on AI Agents to assist them with operational tasks.

In this exercise, you will learn how SAP Automation Pilot can expose operational commands as AI tools through the **Model Context Protocol (MCP)** and how these tools can be consumed by an AI Agent built with SAP Joule Studio.

For a better understanding of the scenario, refer to the diagram below:

![](./images/ex03-dsag-01.png)

---

## Objective

After completing this exercise, you will:

* Understand what MCP (Model Context Protocol) is
* Learn how SAP Automation Pilot can expose commands as AI tools
* Understand how SAP Joule Studio consumes MCP servers
* Explore a preconfigured HANA Cloud Lifecycle Management Agent
* Execute operational tasks using natural language
* Experience AI-enabled Operations for SAP HANA Cloud

---

# What is MCP?

**Model Context Protocol (MCP)** is an open standard that allows AI Agents and Large Language Models to securely interact with external tools and systems.

Think of MCP as:

```text
REST APIs for AI Agents
```

Instead of an AI Agent generating only text, MCP allows the Agent to:

* Discover available tools
* Understand their purpose
* Execute them
* Receive structured responses

For operations teams this means:

```text
Operator
    |
Natural Language
    |
    v
Joule Agent
    |
MCP Server
    |
SAP Automation Pilot
    |
SAP HANA Cloud
```

The AI Agent becomes capable of performing operational activities on behalf of the operator.

---

# SAP Automation Pilot as an MCP Server

SAP Automation Pilot can expose commands as MCP tools.

For example, the command:

```text
GetHanaCloudBackup
```

can be exposed as a tool with:

| Property       | Value       |
| -------------- | ----------- |
| Execution Mode | Synchronous |
| Type           | Read-Only   |

This allows an AI Agent to execute the command and immediately receive the result.

The following documentation explains the integration in more detail:

* Integrating the Service with AI Agents
* Integrating the Service with Joule Studio

---

# Available SAP HANA Cloud Lifecycle Management Commands

SAP provides a large collection of SAP HANA Cloud lifecycle management commands which can be exposed through MCP.

Repository:

https://github.com/SAP-samples/automation-pilot-examples/tree/main/hana-lifecycle-management

Examples include:

* HANA Instance Lifecycle Management
* Backup and Recovery
* Snapshot Management
* Clone Templates
* HANA Upgrades
* Plugin Management
* Elastic Compute Nodes (ECNs)
* Instance Configuration
* Availability Checks
* Audit Log Analysis

These commands can easily be extended to cover virtually any SAP HANA Cloud Day-2 Operations scenario.

---

# Exercise 3.1 – Create Your First MCP Tool

To understand the concept, let's review a simple example.

Open the command:

```text
GetHanaCloudBackup
```

Within SAP Automation Pilot, the command can be exposed through MCP by configuring:

| Setting        | Value              |
| -------------- | ------------------ |
| Tool Name      | GetHanaCloudBackup |
| Execution Mode | Synchronous        |
| Read Only      | Enabled            |

This allows AI Agents to safely execute the command and retrieve information about the latest database backup.

Because the command is marked as Read-Only, the AI Agent cannot modify any resources.

---

# Exercise 3.2 – Explore the Preconfigured MCP Server

To save time during the workshop, a dedicated MCP server has already been created for you.

The MCP server is called:

```text
HANA Cloud Lifecycle Manager
```

It exposes the following operational capabilities:

* List HANA Cloud Instances
* Get HANA Instance Details
* List Available Upgrade Versions
* Check HANA Instance Availability
* Check HANA Cloud Backup Status
* List HANA Cloud Snapshots
* Create HANA Cloud Snapshot
* Check HANA Cloud Audit Logs

These capabilities are intentionally configured as read-only because all participants work with a shared SAP HANA Cloud instance.

![](./images/ex03-01.png)

---

# Exercise 3.3 – Explore the Agent in Joule Studio

The MCP server has already been integrated into Joule Studio.

A preconfigured Agent named:

```text
HANA Lifecycle Operations
```

has already been deployed.

The Agent is based on:

* SAP Joule Studio
* SAP Automation Pilot
* MCP
* Anthropic Claude Sonnet 4

The Agent has access to all MCP tools exposed by the HANA Cloud Lifecycle Manager MCP server.

---

## Agent Purpose

The Agent acts as a virtual SAP HANA Cloud Database Administrator.

It can:

* Discover HANA Cloud instances
* Analyze health status
* Review backups
* Analyze audit logs
* Review snapshots
* Prepare operational recommendations

while following operational guardrails defined by the operations team.

---

# Exercise 3.4 – Interact with the Agent

Open the Agent using the URL provided by the instructor.

Login using:

```text
XP267-0XX@education.cloud.sap
```

and the password provided during the workshop.

---

## Sample Questions

Try the following prompts:

### Instance Discovery

```text
Hey Joule, can you list my HANA instances?
```

### Health Assessment

```text
Make a health status report for my hana-other
```

### Recent Operations

```text
Which are its last operations?
```

### Snapshot Management

```text
Can you list all snapshots for this instance?
```

### Snapshot Status

```text
Check the current status for the snapshot in place.
```

### Audit Log Analysis

```text
Can you check HANA Cloud database for any suspicious audit logs in the last day?
```

### Backup Verification

```text
Give me details about the last DB backup for my hana-other.
```

### Health Rating

```text
Rate the health status (1 to 10) of this instance.
```

### Health Rating with Explanation

```text
Rate the health status (1 to 10) for this instance and provide explanation about your assessment.
```

---

## Bonus Challenges

Try one of the following questions:

```text
Do you find any critical concerns about my HANA Cloud running on Other Environment?
```

```text
Can you prepare an update plan for my HANA Cloud instance running on Other Environment?
```

You are also encouraged to try your own operational questions.

> Please avoid destructive actions such as upgrades, instance deletion, recovery operations, or stop/start activities during the workshop.

---

# Why Is This Important?

Traditionally, operators must:

```text
Open Monitoring Tools
↓
Collect Data
↓
Analyze Results
↓
Decide Next Action
```

With AI-enabled Operations:

```text
Ask a Question
↓
Agent Executes Tools
↓
Agent Collects Evidence
↓
Agent Provides Recommendation
```

The operator remains in control while the AI Agent accelerates operational analysis and decision making.

---

## Summary

You have successfully:

* Learned the fundamentals of MCP
* Understood how SAP Automation Pilot exposes commands as AI tools
* Explored MCP integration with SAP Joule Studio
* Used a HANA Cloud Lifecycle Management Agent
* Executed operational tasks using natural language
* Experienced AI-enabled Operations for SAP HANA Cloud

Congratulations! You have completed the hands-on workshop **"SAP HANA Cloud Remediations in Action with SAP Automation Pilot"**.
