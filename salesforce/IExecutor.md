# IExecutor Interface

Interface for executing rule evaluations. The standard executor is stateless and you can create a new executor with [`RuleEval.executor()`](RuleEval.md#executor) for each rule execution.

## Namespace 

`RuleEval`

## Examples

```apex
// Example: Get an executor
RuleEval.IExecutor myExecutor = RuleEval.executor();
```

```apex
// Example: Execute multiple RuleEval.EvalInstances at once.
Account acc = new Account(Name='Clifford');
Account acc2 = new Account(Name='Clifford');

RuleEval.EvalInstance request = RuleEval.builder()
    .withRule('AccountRule')
    .withPath('Root/Account')
    .withInput(acc)
    .build();

	RuleEval.EvalInstance request2 = RuleEval.builder()
    .withRule('AccountRule')
    .withPath('Root/Account')
    .withInput(acc)
    .build();

RuleEval.executor().evaluate(new List<RuleEval.EvalInstance> {request, request2}, false);
```

## Usage ##

Use this interface to execute your evaluation requests.

## IExecutor Methods

The following are methods for [`RuleEval.IExecutor`](IExecutor.md).

- [`evaluate(RuleEval.EvalInstance instance)`](#evaluateruleevalevalinstance-instance)  
Evaluates a single instance. Throws an `RuleEval.RuleEvalException` if the rule is not executed successfully.

- [`evaluate(List<RuleEval.EvalInstance> instances)`](#evaluatelistruleevalevalinstance-instances)  
Evaluates a batch of instances. Throws an `RuleEval.RuleEvalException` if at least one rule is not executed successfully.

- **(Experimental)** [`evaluate(RuleEval.EvalInstance instance, Boolean allOrNone)`](#evaluateruleevalevalinstance-instance-boolean-allornone)  
Evaluates a single instance. If `allOrNone` is true, a `RuleEval.RuleEvalException` is thrown on a rule failure, otherwise the execution result can be accessed for each [`RuleEval.EvalInstance`](EvalInstance.md) with `isSuccess()`.

- **(Experimental)** [`evaluate(List<RuleEval.EvalInstance> instances, Boolean allOrNone)`](#evaluatelistruleevalevalinstance-instances-boolean-allornone)  
Evaluates a batch of instances. If `allOrNone` is true, a `RuleEval.RuleEvalException` is thrown on any rule failure, otherwise the execution result can be accessed for each [`RuleEval.EvalInstance`](EvalInstance.md) with `isSuccess()`.


---


### `evaluate(RuleEval.EvalInstance instance)`

Evaluates a single instance. Throws an `RuleEval.RuleEvalException` if the rule is not executed successfully.  

**Signature**

```apex
global void evaluate(RuleEval.EvalInstance instance)
```

**Examples**

```apex
RuleEval.EvalInstance request;
...
RuleEval.executor().evaluate(request);
System.debug(request.isSuccess());
// debug: true
```


---


### `evaluate(List<RuleEval.EvalInstance> instances)`

Evaluates a batch of instances. Throws an `RuleEval.RuleEvalException` if at least one rule is not executed successfully.  

**Signature**

```apex
global void evaluate(List<EvalInstance> instances)
```

**Examples**

```apex
RuleEval.EvalInstance request;
RuleEval.EvalInstance request2;
List<RuleEval.EvalInstance> requests = new List<RuleEval.EvalInstance> {request, request2};
...
RuleEval.executor().evaluate(requests);
System.debug(request.isSuccess());
// debug: true
System.debug(request2.isSuccess());
// debug: true
```


---


### `evaluate(RuleEval.EvalInstance instance, Boolean allOrNone)`

> **Note: This is an experimental feature, planned for future releases of the Rule Engine.**  
> **The method may not work as expected and should only be used for test purposes.**

Evaluates a single instance. If `allOrNone` is true, an `RuleEval.RuleEvalException` is thrown on a rule failure, otherwise the execution result can be accessed for each [`RuleEval.EvalInstance`](EvalInstance.md) with `myEvalInstance.isSuccess()`.  

**Signature**

```apex
global void evaluate(RuleEval.EvalInstance instance)
```

**Examples**

```apex
RuleEval.EvalInstance request;
...
RuleEval.executor().evaluate(request, false);
System.debug(request.isSuccess());
// debug: false
```


---

### `evaluate(List<RuleEval.EvalInstance> instances, Boolean allOrNone)`

> **Note: This is an experimental feature, planned for future releases of the Rule Engine.**  
> **The method may not work as expected and should only be used for test purposes.**

Evaluates a batch of instances. If `allOrNone` is true, a `RuleEval.RuleEvalException` is thrown on any rule failure, otherwise the execution result can be accessed for each [`RuleEval.EvalInstance`](EvalInstance.md) with `myEvalInstance.isSuccess()`.

**Signature**

```apex
global void evaluate(List<RuleEval.EvalInstance> instances, Boolean allOrNone)
```

**Examples**

```apex
RuleEval.EvalInstance request;
RuleEval.EvalInstance request2;
List<RuleEval.EvalInstance> requests = new List<RuleEval.EvalInstance> {request, request2};
...
RuleEval.executor().evaluate(requests, false);
System.debug(request.isSuccess());
// debug: true
System.debug(request2.isSuccess());
// debug: false
```


---