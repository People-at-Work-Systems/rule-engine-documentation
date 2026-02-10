# RuleEval Class

The `RuleEval` class is the primary entry point for the Rule Engine framework. It provides a fluent API to build rule evaluation requests and an execution layer to process these requests via remote API callouts.

## Namespace 

`RuleEval`

## Examples

```apex
// Example: Evaluating a single rule
RuleEval.EvalInstance request = RuleEval.builder()
    .withRule('DiscountRule')
    .withInput(new Map<String, Object>{ 'amount' => 100 })
    .withReturnType(Decimal.class)
    .build();

RuleEval.executor().evaluate(request);

if (request.isSuccess()) {
    Decimal result = (Decimal)request.getSingleLineResult();
    System.debug('Rule result: ' + result);
}
```

## Usage ##

Use this class to construct evaluation instances and execute them. The framework supports single and bulk evaluations with automatic JSON deserialization of results.

## RuleEval Methods

The following are methods for `RuleEval`.

- [`builder()`](#builder)  
Returns a new instance of the [`RuleEval.Builder`](Builder.md) class to construct an evaluation request.

- [`executor()`](#executor)  
Returns an instance of `IExecutor` used to process rule evaluations. By default, it returns a standard [`RuleEval.IExecutor`](IExecutor.md) implementation but can return a mock during unit tests.

- [`setMockExecutor(IExecutor mockExecutor)`](#setmockexecutoriexecutor-mockexecutor)  
A private @TestVisible method to mock the standard [`RuleEval.IExecutor`](IExecutor.md) during test runs.


---


### `builder()`

Returns a new instance of the [`RuleEval.Builder`](Builder.md) class to construct an evaluation request.

**Signature**

```apex
global static RuleEval.Builder builder()
```

**Return Value**

Type: [`RuleEval.Builder`](Builder.md)


---


### `executor()`

Returns an instance of [`RuleEval.IExecutor`](IExecutor.md) used to process rule evaluations. By default, it returns a standard implementation but can return a mock during unit tests.

**Signature**

```apex
global static RuleEval.IExecutor executor()
```

**Return Value**

Type: [`RuleEval.IExecutor`](IExecutor.md)


---


### `setMockExecutor(IExecutor mockExecutor)`

A private @TestVisible method to mock the standard [`RuleEval.IExecutor`](IExecutor.md) during test runs.

**Signature**

```apex
private static setMockExecutor(IExecutor mockExecutor)
```

**Parameters**

***mockExecutor***

A class that implements the [`RuleEval.IExecutor`](IExecutor.md) interface.

**Example**

```apex
private class MockExecutor implements RuleEval.IExecutor {
    public void evaluate(RuleEval.EvalInstance instance) {
        evaluate(new List<RuleEval.EvalInstance> {instance}, true);
    }

    public void evaluate(RuleEval.EvalInstance instance, Boolean allOrNone) {
        evaluate(new List<RuleEval.EvalInstance> {instance}, true);
    }
    
    public void evaluate(List<RuleEval.EvalInstance> instances) {
        evaluate(instances, true);
    }

    public void evaluate(List<RuleEval.EvalInstance> instances, Boolean allOrNone) {
        for (RuleEval.EvalInstance instance : instances) {
            instance.isSuccess = true;
            instance.output = new List<String> {'a', 'b', 'c'};
        }
    }
}

@IsTest
static void test() {
    ...
    RuleEval.setMockExecutor(new MockExecutor());
    
    Test.startTest();
    ...
    Test.stopTest();       
}
```

---