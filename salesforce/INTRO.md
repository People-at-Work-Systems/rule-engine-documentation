# Introduction

The PAW Rule Engine is a Business Rule Management System.


## Installation

After installing the package, you have to do the following steps:

1. Assign the `Manage Rule Engine` permission set to anyone, who should be able to access the rule engine and access the connection settings.
2. Add the Rule Engine URL to the Remote Site Settings under Setup > Security > Remote Site Settings.
3. Create a Trusted URL under Setup > Security > Trusted URLs. Enter the URL, select `All` as CSP Context and check `frame-src` in the CSP Directives section.
4. Provide the URL in the `Rule Engine App`s Settings Tab.
5. Provide the API Key in the `Rule Engine App`s Settings Tab.


## Documentation Apex API

- [RuleEval Class](RuleEval.md)
- [RuleEval.Builder Class](Builder.md)
- [RuleEval.EvalInstance Class](EvalInstance.md)
- [RuleEval.IExecutor Interface](IExecutor.md)


## Documentation Flow API

The Flow API offers several Apex Actions to execute Rules and parse the results.

[Flow API Documentation](Flow.md)


## Rule Engine Salesforce Connector

The __Rule Engine Salesforce Connector__ is a powerful framework within Salesforce that enables developers to execute business rules dynamically without hard-coding logic. It provides a flexible, reusable approach to rule evaluation with strong integration capabilities for [__Flows__](#documentation-flow-api), [__Apex code__](#documentation-apex-api), and __JSON-based__ rule processing.

Core Capabilities

### 1. Dynamic Rule Evaluation

- Execute predefined business rules by name from an external rule engine service
- Support for single and bulk rule evaluations
- Fluent builder pattern for intuitive rule construction
- Configurable return types with automatic JSON deserialization

### 2. Multiple Integration Patterns

- __Apex Direct API__: Use [`RuleEval`](RuleEval.md) class for programmatic rule execution in Apex code
- __Flow Integration__: Invocable actions provide seamless integration with Salesforce Flows
- __SObject Processing__: Evaluate rules against single or collection of SObjects
- __JSON Processing__: Execute rules against raw JSON data strings

### 3. Primary Entry Points

__RuleEval Class__

- [`RuleEval.builder()`](RuleEval.md#builder) - Creates a builder for constructing evaluation instances
- [`RuleEval.executor()`](RuleEval.md#executor) - Returns an executor for processing rule evaluations
- Supports mocking for unit testing

__Flow-Ready Invocable Actions__

- __RuleExecutionAction__: Evaluate a rule for a single SObject
- __RuleExecutionCollectionAction__: Evaluate a rule against multiple SObjects in a collection
- __RuleExecutionJSONAction__: Evaluate a rule for a single JSON input
- __RuleExecutionJSONCollection__: Evaluate rules against multiple JSON inputs

### 4. Data Handling

__Input Types__

- SObject instances (single or collection)
- JSON strings
- Custom Apex objects (via Map or direct object serialization)


__Output Processing__

- Single-line results
- Multi-line results
- Type-safe deserialization with configurable return types
- Success/failure status tracking

### 5. Advanced Features

- __JSON Parsing__: Built-in JSON extraction utilities for Flow Builder
- __HTTP Integration__: Callout support for remote rule engine services
- __Configuration Management__: Rule Engine App for managing rule engine settings
- __Batch Processing__: Bulk evaluation with transaction control options
- __Path Resolution__: Optional path parameter for rule location specification

### 6. Developer Experience

```java
// Custom Apex Return type
public class Result {
    public String code;
    public Decimal discount;
    public Integer row_number;
}

// Simple fluent API usage
RuleEval.EvalInstance request = RuleEval.builder()
    .withRule('DiscountRule')
    .withInput(accountRecord)
    .withPath('rules/financial')
    .withReturnType(Result.class)
    .build();

RuleEval.executor().evaluate(request);

Result r = (Result) request.getSingleLineResult();
String code = r.code;
Decimal discount = r.discount;
Integer row_number = r.row_number;
```

Key Benefits

- __Separation of Concerns__: Business logic decoupled from Salesforce code
- __Reusability__: Single rule definition used across Flows, Apex, and integrations
- __Testability__: Mock executor support for comprehensive unit testing
- __Scalability__: Bulk evaluation capabilities for performance optimization
- __Flexibility__: Support for various input/output formats and rule definitions

This API is designed for organizations requiring dynamic, centrally-managed business rule execution while maintaining clean, maintainable code architecture.
