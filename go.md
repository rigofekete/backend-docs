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

In Go, the `comma OK idiom` can be used for map lookups:

```go 
_, ok := someMap["ouch"]
if !ok {
    //
}
```

### Comma OK idiom

In Go, this pattern can be used in 3 situations:

- Map lookups
- Type assertions
- Channel receives  

## Type Assertion 

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

## Enums

The Go language does have an `enum` keyword. However, this is the idiomatic way to implement it:

```go
type Colors int

const (
    Red        Colors = iota
    Blue
    Yellow
    Black
)
```

`Iota` is a special keyword in Go that creates a sequence of numbers, starting from 0 and incrementing by one for every declared symbolic constant line in the `const` block.

This gives a something that resembles an `Enum`, which is a collection of named (symbolic constants) integers. 

### Exported names

It is very important to remember that names (variables, types, functions) are only available outside of the `package`, when the first character is capitalized. All of the other names are `private` and `encapsulated` inside the declared `package` only.   

```go
package rigo

// Only available in the rigo package
type fekete struct {
    // also private
    name string
}

// Exported name, available outside of the rigo package
type Piros struct {
    // Also exported
    Name string
}
```

## Interfaces 

Interfaces in Go are somewhat similar to interface classes in C++, in the sense that they only contain method signatures. These functions resemble `pure virtual functions` since they are plain signatures without any body.

`Interfaces` set the basic behavior that other types can override, by implementing all of their declared methods. When a `type` implements an `interface` (defines all of its `method signatures`) it can be used as that `interface` type anywhere the code expects to use it or receive it. 

This allow for code to be reused more cleanly, for example, any function that expect to receive that `interface` as an argument, can receive many different forms of itself (`polymorphism`), each one written to behave in a specific way.   

It is also allowed to embed `interfaces` alongside the method signatures. This is possible because `embedding` in Go (structs or interfaces) spread the `properties` into the current scope. Since `interfaces` have nothing other than `method signatures` inside, this is valid. 



##  Concurrency

One of the main features of the Go language is how nicely it handles `concurrency`.

 The first advantage is that its concurrent feature, `go routine`, is lightweight in comparison with `  OS threads`, since Go can spawn multiple of these lightweight concurrent executions using resources from 1 single `OS thread`. 

Another advantage is that using the same concurrency code (the `go` keyword) can either execute tasks `asynchronously` when running on a single core CPU or in `parallel` with multiple core processors.   

## Channels

## Generics
