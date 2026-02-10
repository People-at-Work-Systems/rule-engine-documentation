# EvaluationOutput Apex Class

## Overview
`EvaluationOutput` is an **Apex-Defined Data Type** that serves as the structured response container for the generic JSON rule execution engine or collection actions within **Salesforce Flow**. 

This class is designed to handle complex data returns by encapsulating JSON-formatted strings, allowing Flows to receive and process flexible data structures dynamically.

---

## Flow Configuration
When using this class in Flow Builder, administrators can access the following properties to handle the rule engine's findings.

### Field Mapping

| Property | Data Type | Description |
| :--- | :--- | :--- |
| `ruleName` | String | The name of the rule that was executed. |
| `singleLineResult` | String | **JSON String.** The primary result of the evaluation. This is identical to the first element of the `multiLineResult` collection. |
| `multiLineResult` | List<String> | **Collection of JSON Strings.** A list where each entry is a JSON-formatted string representing an individual result item. |

---

## Technical Logic & Implementation

### JSON Result Structure
Unlike standard strings, the `singleLineResult` and the items within `multiLineResult` are **JSON-formatted**. To use this data effectively in Flow:
1. Use an **Apex Action** (see the Extract Actions inside this package) to transform these strings into Flow variables.
2. If only one result is expected, use `singleLineResult` directly.
3. If multiple results are expected, use a **Loop** element to iterate through the `multiLineResult` collection.

### Relationship between Results
The class follows a "First-Entry" logic for convenience:
> **Note:** `singleLineResult` is always equal to `multiLineResult[0]`. 
> This allows the Flow to quickly access the primary result without needing a Loop element if only the first or most relevant result is required.