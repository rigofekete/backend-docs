# GO

## Slices

Slices in Go are dynamically sized collections of ordered elements. They are references to underlying arrays, which hold the data in memory. A slice is a pointer to a portion of this underlying array. 

Because they are dynamically sized, as opposed to arrays, whenever a new element is appended to it, an internal check for available capacity will take place (on the underlying array) and if more capacity is needed, a new allocation of memory will be done for a new array, which will become the new underlying memory space that the slice will point to.

Besides slicing an existing declared array, we can initialize slices directly with the `make` function, which will allow us to set the `length` of the slice and the `capacity` of the underlying array: 

```go 
make([]int, 5, 10)
```

## Maps

Maps in Go are `dictionaries` (`hash maps`) that store `key/value` pairs. Only comparable types are allowed to be used as `keys`. 

There is also an idiomatic way to confirm existence of map keys in Go, called the `comma ok idiom` :

```go 
_, ok := someMap["ouch"]
if !ok {
    //
}
```

## Interfaces 

Interfaces in Go are somewhat similar to interface classes in C++, in the sense that they only contain method signatures. These functions resemble `pure virtual functions` since they are plain signatures without any body.

`Interfaces` set the basic behavior that other types can override, by implementing all of their declared methods. When a `type` implements an `interface` (defines all of its `method signatures`) it can be used as that `interface` type anywhere the code expects to use it or receive it. 

This allow for code to be reused more cleanly, for example, any function that expect to receive that `interface` as an argument, can receive many different forms of itself (`polymorphism`), each one written to behave in a specific way.   

It is also allowed to embed `interfaces` alongside the method signatures. This is possible because `embedding` in Go (structs or interfaces) spread the `properties` into the current scope. Since `interfaces` have nothing other than `method signatures` inside, this is valid. 

### Type Assertion 

We can verify the type of an `interface` value by doing a `type assertion`:

```go
func (v animal) {
    value, ok := v.(dog)
    if ok {
        // use the dog type logic
    }
}
```

If multiple checks are needed, we can `type switch` on a `type assertion`: 

```go
switch value := v.(type) {
    case dog:
        // 
    case cat:
        // 
}
```

##  Concurrency

One of the main features of the Go language is how nicely it handles `concurrency`.

 The first advantage is that its concurrent feature, `go routine`, is lightweight in comparison with `  OS threads`, since Go can spawn multiple of these lightweight concurrent executions using resources from 1 single `OS thread`. 

Another advantage is that using the same concurrency code (the `go` keyword) can either execute tasks `asynchronously` when running on a single core CPU or in `parallel` with multiple core processors.   

