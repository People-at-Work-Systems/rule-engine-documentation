# EvaluationInput Apex Class

## Overview
`EvaluationInput` is an **Apex-Defined Data Type** designed to bridge the gap between Salesforce Flows and the generic JSON rule execution engine. It serves as a structured container, allowing Flow administrators to pass complex data parameters into rule-processing logic without writing custom Apex code for every transaction.

---

## Flow Configuration
To use this class within the **Salesforce Flow Builder**, follow these specifications:

### Field Mapping
When you define a variable using this Apex-Defined class, the following properties are available for assignment:

| Property | Data Type | Description |
| :--- | :--- | :--- |
| `ruleName` | String | The unique identifier/API name of the business rule to be triggered. |
| `json` | String | The data payload formatted as a JSON string that will be evaluated. |
| `path` | String | (Optional) The specific JSON path or target node to be processed within the payload. |

---

## Implementation Guide

### 1. Variable Definition
1. Open Flow Builder.
2. Create a **New Resource**.
3. Set **Resource Type** to `Variable`.
4. Set **Data Type** to `Apex-Defined`.
5. Select `EvaluationInput` as the **Apex Class**.

### 2. Setting Values
Use an **Assignment** element within your Flow to map values to the variable. 
* Example: `var_EvaluationInput.ruleName` ← "Tax_Calculation_Rule"

### 3. Execution
Pass the populated variable as an input parameter to the **Apex Action** responsible for the rule evaluation.