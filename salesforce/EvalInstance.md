# EvalInstance Class

Represents the data container for a rule evaluation, holding both the request parameters and the execution results. [`RuleEval.EvalInstance`](EvalInstance.md)s are created with the [`RuleEval.Builder`](Builder.md) class.

## Namespace 

`RuleEval`

## Examples

```apex
// Example: Raw Output of the Rule Engine with a Multiline Rule
Account acc = new Account(Name='Clifford');

RuleEval.EvalInstance request = RuleEval.builder()
    .withRule('AccountRule')
    .withPath('Root/Account')
    .withInput(acc)
    .build();

RuleEval.executor().evaluate(request);

System.debug(JSON.serializePretty(request.getOutput()));
```

```json
[
	{
		"code": "winter_sale",
		"discount": 5,
		"row_number": 2
	},
	{
		"code": "newsletter",
		"discount": 10,
		"row_number": 5
	},
	{
		"code": "cash",
		"discount": 3,
		"row_number": 6
	}
]
```

## Usage ##

Use this class to wrap your rule evaluation request and process the execution results.

## EvalInstance Methods

The following are methods for [`RuleEval.EvalInstance`](EvalInstance.md).

- [`isSuccess()`](#issuccess)  
Indicates whether the rule was evaluated successfully without errors.

- [`getInput()`](#getinput)  
Returns the provided input.

- [`getRuleName()`](#getrulename)  
Returns the provided rule name.

- [`getOutput()`](#getoutput)  
Returns the raw output of the rule evaluation.

- [`getSingleLineResult()`](#getsinglelineresult)  
A convenience method that returns the first element of the result list. Use this when the rule is expected to return a single line and not a multi line result.

- [`getMultiLineResult()`](#getmultilineresult)  
A method that returns the result list. Use this when the rule is a multi line rule, that returns multiple rows of data.

- [`getNumberOfRows()`](#getnumberofrows)  
Returns the number of result rows returned by the rule.

- [`hasResult()`](#hasresult)  
Returns true if there was at least one matching line in the evluation rule.

- [`isEvaluated()`](#isevaluated)  
Returns true if this instance was sent to the rule engine for evaluation.


---


### `isSuccess()`

Indicates whether the rule was evaluated successfully without errors.

**Signature**

```apex
global Boolean isSuccess()
```

**Return Value**

Type: `Boolean`.

Throws a `RuleEval.Unevaluated` exception if this [`RuleEval.EvalInstance`](EvalInstance.md) was not yet evaluated.


---


### `getInput()`

Returns the provided input.

**Signature**

```apex
global Object getInput()
```

**Return Value**

Type: `Object`


---


### `getRuleName()`

Returns the provided rule name.

**Signature**

```apex
global String getRuleName()
```

**Return Value**

Type: `String`


---


### `getOutput()`

Sets the name of the rule to be executed.

**Signature**

```apex
global Object getOutput()
```

**Return Value**

Type: `Object`

The raw output of the rule execution in the form of an untyped deserialized json. 

Throws a `RuleEval.Unevaluated` exception if this [`RuleEval.EvalInstance`](EvalInstance.md) was not yet evaluated.

**Examples**

JSON Raw output:
```json
[
	{
		"code": "winter_sale",
		"discount": 5,
		"row_number": 2
	},
	{
		"code": "newsletter",
		"discount": 10,
		"row_number": 5
	},
	{
		"code": "cash",
		"discount": 3,
		"row_number": 6
	}
]
```

```apex
RuleEval.EvalInstance request;
...
List<Object> lines = (List<Object>) request.getOutput();
Map<String, Object> firstLine = (Map<String, Object>) lines[0];
System.debug(firstline.get('code'));

// debug: "winter_sale"
```

---


### `getSingleLineResult()`

A convenience method that returns the first element of the result list. Use this when the rule is expected to return a single line and not a multi line result.

**Signature**

```apex
global Object getSingleLineResult()
```

**Return Value**

Type: `Object`

The single line output. If a Return Type was provided, then the `Object` can be casted to it, otherwise an untyped deserialzed json is returned.

Throws a `RuleEval.Unevaluated` exception if this [`RuleEval.EvalInstance`](EvalInstance.md) was not yet evaluated.

**Examples**

JSON:
```json
{
    "code": "winter_sale",
    "discount": 5,
    "row_number": 2
}
```

```apex
RuleEval.EvalInstance request;
...
MyApexType output = (MyApexType) request.getSingleLineResult();
System.debug(output.code);

// debug: "winter_sale"
```

---


### `getMultiLineResult()`

A method that returns the result list. Use this when the rule is a multi line rule, that returns multiple rows of data.  

**Signature**

```apex
global List<Object> getMultiLineResult()
```

**Return Value**

Type: `List<Object>`

If a Return Type was provided, then all `Object`s inside the List can be casted to it, otherwise each object is an untyped deserialized json.

Throws a `RuleEval.Unevaluated` exception if this [`RuleEval.EvalInstance`](EvalInstance.md) was not yet evaluated.

**Examples**

JSON multi line output:
```json
[
	{
		"code": "winter_sale",
		"discount": 5,
		"row_number": 2
	},
	{
		"code": "newsletter",
		"discount": 10,
		"row_number": 5
	},
	{
		"code": "cash",
		"discount": 3,
		"row_number": 6
	}
]
```

```apex
RuleEval.EvalInstance request;
...
for (Object obj : request.getMultiLineResult()) {
    MyApexType output = (MyApexType) obj;
    System.debug(output.discount);
}

// debug: 5
// debug: 10
// debug: 3
```


---


### `getNumberOfRows()`

Returns the number of result rows returned by the rule. Single line rules always return 1.

**Signature**

```apex
global Integer getNumberOfRows()
```

**Return Value**

Type: `Integer`

Throws a `RuleEval.Unevaluated` exception if this [`RuleEval.EvalInstance`](EvalInstance.md) was not yet evaluated.


---


### `hasResult()`

Returns true if there was at least one matching line in the evaluation rule.

**Signature**

```apex
global Boolean hasResult()
```

**Return Value**

Type: `Boolean`

Throws a `RuleEval.Unevaluated` exception if this [`RuleEval.EvalInstance`](EvalInstance.md) was not yet evaluated.


---


### `isEvaluated()`

Returns true if this instance was sent to the rule engine for evaluation.

**Signature**

```apex
global Boolean isEvaluated()
```

**Return Value**

Type: `Boolean`


---