# Builder Class

A helper class to construct an [`RuleEval.EvalInstance`](EvalInstance.md) with a fluent API.

## Namespace 

`RuleEval`

## Examples

```apex
// Example: Create an RuleEval.EvalInstance
RuleEval.EvalInstance request = RuleEval.builder()
    .withRule('DiscountRule')
    .withInput(new Map<String, Object>{ 'amount' => 100 })
    .build();
```

```apex
// Example: Create and evaluate an RuleEval.EvalInstance with a single line rule.
@JsonAccess(deserializable='always')
global class AccountRuleResult {
    global String code;
    global Decimal discount;
    global Integer row_number;
}

Account acc = new Account(Name='Clifford');

RuleEval.EvalInstance request = RuleEval.builder()
    .withRule('AccountRule')
    .withPath('Root/Account')
    .withInput(acc)
    .withReturnType(AccountRuleResult.class)
    .build();

RuleEval.executor().evaluate(request);

AccountRuleResult myResult = (AccountRuleResult) request.getSingleLineResult();
```

## Usage ##

Use this class to construct evaluation instances, that can later on be sent to the remote api.

## Builder Methods

The following are methods for [`RuleEval.Builder`](Builder.md).

- [`withInput(Object input)`](#withinputobject-input)  
Sets the payload to be processed by the rule engine. Can be any serializable Object.

- [`withRule(String ruleName)`](#withrulestring-rulename)  
Sets the name of the rule to be executed.

- [`withPath(String path)`](#withpathstring-path)  
Optional. Sets a specific folder path for the rule, in case the rule is not in the default unassigned folder.

- [`withReturnType(Type returnType)`](#withreturntypetype-returntype)  
Optional. Sets the Apex Type for automatic JSON deserialization of the result. If not set, an untyped deserialized json is returned.

- [`build()`](#build)  
Validates and returns the [`RuleEval.EvalInstance`](EvalInstance.md). Throws `RuleEval.RuleEvalException` if input or ruleName is missing.


---


### `withInput(Object input)`

Sets the payload to be processed by the rule engine. Can be any serializable Object.

**Signature**

```apex
global Builder withInput(Object input)
```

**Parameters**

***input***

Any serializable Object.

**Return Value**

Type: [`RuleEval.Builder`](Builder.md)

**Examples**

```apex
// Custom Json input
RuleEval.EvalInstance request = RuleEval.builder()
    .withRule('AccountDiscountRule')
    .withInput(new Map<String, Object>{ 'Name' => 'Clifford', 'NumberOfEmployees' => 250 })
    .build();
```

```apex
// SObject input
RuleEval.EvalInstance request = RuleEval.builder()
    .withRule('AccountDiscountRule')
    .withInput(acc)
    .build();
```

---


### `withRule(String ruleName)`

Sets the name of the rule to be executed.

**Signature**

```apex
global Builder withRule(String ruleName)
```

**Parameters**

***ruleName***

The name of the rule.

**Return Value**

Type: [`RuleEval.Builder`](Builder.md)


---


### `withPath(String path)`

Optional. Sets a specific folder path for the rule, in case the rule is not in the default unassigned folder.

**Signature**

```apex
global Builder withPath(String path)
```

**Parameters**

***path***

The path to the rule. Use the following structure `Root\Account\Discount`. If no path is provided, then the rule is searched inside the unassigned folder.

**Return Value**

Type: [`RuleEval.Builder`](Builder.md)


---


### `withReturnType(Type returnType)`

Optional. Sets the Apex Type for automatic JSON deserialization of the result. If not set, an untyped deserialized json is returned.

> **Note:** Your custom apex class and its members must be global.

> **Note:** You must annotate your global class with `@JsonAccess(deserializable='always')`, so that the managed package code can deserialize to your local class.

> **Note:** If you add the `Integer` member `row_number` to your custom apex class,  
> the rule engine will fill this variable with the row number, that evaluated to true.

**Signature**

```apex
global Builder withReturnType(Type returnType)
```

**Parameters**

***returnType***

An Apex class type.

**Return Value**

Type: [`RuleEval.Builder`](Builder.md)

**Examples**

```apex
// Example: Create and evaluate an RuleEval.EvalInstance with a single line rule and get a specific return type.
@JsonAccess(deserializable='always')
global class AccountRuleResult {
    global String code;
    global Decimal discount;
    global Integer row_number;
}

Account acc = new Account(Name='Clifford');

RuleEval.EvalInstance request = RuleEval.builder()
    .withRule('AccountRule')
    .withPath('Root/Account')
    .withInput(acc)
    .withReturnType(AccountRuleResult.class)
    .build();

RuleEval.executor().evaluate(request);

AccountRuleResult myResult = (AccountRuleResult) request.getSingleLineResult();
```

```apex
// Example: Create and evaluate an RuleEval.EvalInstance with a single line rule and get an untyped result. See example above for the output structure.
Account acc = new Account(Name='Clifford');

RuleEval.EvalInstance request = RuleEval.builder()
    .withRule('AccountRule')
    .withPath('Root/Account')
    .withInput(acc)
    .build();

RuleEval.executor().evaluate(request);

Map<String, Object> myResult = (Map<String, Object> myResult) request.getSingleLineResult();
System.debug(myResult.get('code'));
System.debug(myResult.get('discount'));
System.debug(myResult.get('row_number'));
```

---


### `build()`

Validates and returns the [`RuleEval.EvalInstance`](EvalInstance.md). Throws `RuleEval.RuleEvalException` if input or ruleName is missing.

**Signature**

```apex
global RuleEval.EvalInstance build()
```

**Return Value**

Type: [`RuleEval.EvalInstance`](EvalInstance.md)


---