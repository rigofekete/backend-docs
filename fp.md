# Functional Programming

<br>

## What is Functional Programming?

Functional Programming is a programming paradigm, like others such as `OOP`, which aims to produce more readable and clean code that is easier to debug and maintain.

The main drive of this style of programming is to build code that uses a `declarative` approach as opposed to the `imperative` nature used in other paradigms which imply sequences of instructions or commands. 

`Declarative` in this context means, no `mutation` of variables and state (attributes) of current objects, but rather, producing changes through a successive call of `pure functions`, which will then output new results as new copies. For example, if one needs to change an object of a User class, this will not be done directly or by calling class methods to change each of its attributes. In `Functional Programming`, the changes will occur by calling `pure` functions that will return new object `copies` with all of the changes in place. Internal `mutation` of values does not occur but instead, `copies` with new values will be returned. 

The functions used to produce results in `Functional Programming` are `pure` in the sense that they will return the same output for each equal input passed in (`deterministic`), without any `side-effects`. 



### Additional Notes on Pure Functions

`Pure` functions return the exact same output for any equal input passed in. This brings us to another concept named `Referential Transparency`. 

If a `pure` function is `deterministic` and without `side-effects`, we can safely replace it by the returned `value` directly.

When we already know the output of calling a `pure` function with value `n`, we don't need to repeatedly execute the same function due to its `Referential Transparency` condition. 

In `FP`, this can be improved even further, in a more sophisticated way, through the use of `Memoization`. This technique consists of caching those returned values for later use, whenever the input happens to be already present in cache storage. 


## First-Class Functions

Functions that can be treated as plain values, allowing themselves to be assigned to variables or even passed to other functions as arguments, which generally means they will be treated as `callback` functions (a function that will be called and executed inside the callee function at some step).

## Higher-Order Functions

They receive functions as arguments (`First-Class Functions`) and/or return functions as values. 

These two concepts are crucial in `Functional Programming` when using  functions such as Python `Maps` or `Filters`, where each element of a container, like an array for example, is transformed according to the logic of a function passed in as argument, which can be either a previously defined named function or an `anonymous` one, defined in place. 

## Anonymous Functions 

Also known as `Lambda`, these are unnamed inlined functions that execute very concise operations. In the previous example, I mentioned maps and filters because many times, both of those use `Lambda` functions as arguments to perform algorithmic operations on each element of a given container. 

The use of these different function styles, allow for `Functional Programming` to shine, since they are used to transform or return values within the scope of `immutability`, maintaining the use of `pure functions` in place.  

I have referenced `map` and `filter` so I might as well describe those `Higher-Order Functions` as well as the `reduce` one, below.

Final note to point out that the use of `lambdas` might not be appropriate to comply with `Clean Code` since it forces the programmer to write the inlined function definition multiple times. To make the flow follow a more `clean` and `DRY` approach, we would probably avoid writing `lambdas` and define specific named functions instead, for reuse. 

### Map

Returns a new copy of an existing container, where each element is transformed based on the function passed as argument. 

For example, if we have an array that needs a copy of itself with all of its elements multiplied by x. 

### Filter

Also returns a copy of a given container or collection of values, but this time, filtering the resulting container to hold only the elements that match the conditions defined by the `First Order Function`.

The important detail here is that the original container is never `mutated` but rather transformed and returned as new `copy`. 

### Reduce

Receives a function and a collection of values as arguments, applies the function to each element of the collection but instead of returning a new container object, it accumulates each computation in a single value result.  

<br><br>

These 3 examples of `Higher-Order Functions` show not only how `immutability` remains preserved (by always returning new copies) but also, how `imperative` commands such as `loops` can also be substituted altogether. `Map`, `Filter` and `Reduce` transform elements of a container, one by one. 

## Recursion 

`Loops`, which are by nature statements that not only mutate values, like the index itself, but are also built as a sequence of instructions (`imperative`), can also be replaced by `recursion` in `Functional Programming`. 

`Recursion` is a method that consists of a function calling itself multiple times until hitting a base case. 

At each level of recursion, a transformed value is passed in, until hitting a base case that will subsequently unwind the stack, returning from every function call and passing the return value back through them until the very first caller (`LIFO`). 

A simple example is finding out the factorial of number `n`. 

We need to call a function like factorial(`n`), which will call itself again with a one liner return like this:


```go
return n * factorial(n - 1)
```

and the base case for this function would be: 


```go
if n == 0 {
    return 1
}
```

After hitting this base case, the return value will be multiplied by the current level `n` of each stack frame, subsequently returned and computed until the stack unwinding is complete, returning the final value as the product of all positive numbers <= `n`. 

## Function Transformations

Subtype of `Higher-Order Functions` that always return new functions, hence the term `transformation`. 

This technique is valuable in `FP` because it allows for code to be reused in contexts where we only want one main function (`factory` function) to modulate behavior of new functions, based on the input passed in, which can be either data or function values.

## Closures

A `Closure` is a technique of enclosing existing data that lives outside the function body, such that it will be part of the function, whenever it is called. We can say that a function with a `closure` is `stateful` since it keeps track of enclosed data in itself, which can be updated if needed.  

In the context of `Function Transformations` for example, we can return a transformed function with a `closure` on an array that lives outside the inner function definition. This new returned function object will keep this array `encapsulated` and as part of its state, allowing its internal logic to update it.

## Currying 

Method of decomposing functions that accept multiple arguments, into a sequence of nested functions that take 1 argument each in order to achieve the same logic. 

It is a type of `Function Transformation` that refactors a given function with multiple arguments, by implementing and defining separate inner functions, each one responsible for its own task corresponding to the separated argument. 

This can be handy in situations where a given `Higher-Order Function` expects a `callback` function that only takes 1 argument. If we really need to pass a specific function that has multiple arguments to act as the `callback`, we can `curry` it, transforming it into a 1 arg function without changing any of its logic. 

## Sum Types

`Sum Types`, also know as `Tagged Unions` and `Discriminated Unions`, is a data modelling concept that represents a finite number of variants (states), allowing only 1  of those variants to exist. 

A simple example of `Sum Types` is the `Boolean` type, which has 2 possibilities, `True` and `False`, only allowing one of them to be set. 

A more realistic and slightly complex representation of this would be the case of having different variants, with each one representing a state containing multiple possible type members, as opposed to the previous example of a `Boolean` which only has two variants without any internal data. 

With `Sum Type` we can validate the state of each variant without the verbosity of having to check each possible state combinations and correctness, with the use of long if/else statements. This individual representation of each possible combinations is of the nature of `Product Types`, where each possible value of a variant needs to be multiplied with each possible value of other variants, which can  end up resulting in a very big or even endless chain of combinations. This is useful only to expose the totally of possible states. 

`Sum Types` handles this more graciously, by summing the possibilities of each variant, mainly because we only want one of these states to exist and to be validated, not the totality of options.

What happens with this `Sum Type` method is that we will have either a `switch` or a simple `conditional block`, checking if a certain object has one of the defined valid `states` (which can be checked either through `enum` values or class instance validation), only allowing these and discarding any unknown/undefined ones. 

Besides making the code more readable and less verbose, it also has the positive side-effect of reducing the possibility of bugs, not only because there are less conditional checks but also because only valid states are allowed.   















