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

This gives us something that resembles an `Enum`, which is a collection of named (symbolic constants) integers. 

## Exported names

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

 The first advantage is that its concurrent feature, `goroutine`, is lightweight in comparison with `  OS threads`, since Go can spawn and manage multiple of these lightweight concurrent executions using resources from a small number of `OS threads`. 

Another advantage is that using the same concurrency code (the `go` keyword) can either execute tasks `asynchronously` when running on a single core CPU or in `parallel` with multiple core processors.   

## Channels

`Thread-safe` queues used to send typed data between `channel senders` and `channel receivers`. 

Used in concurrency to allow different `goroutines` to send/receive data between them. 

When data is sent to an `unbuffered` channel sender, the operation will block until a channel receiver gets the item. 

Creating a channel:

```go
ch := make(chan int)
```

A created channel is `bidirectional` by default. It is both a sender\receiver. 

It will be then passed in (and `casted`) as a `<-chan int` or `chan-> int` type, depending of the necessity. 

#### Unbuffered Channel

Default channel that allows only one item to be sent until blocking (waiting for receiver). 

#### Buffered Channel

A channel created with a buffer length. If we have a channel with a buffer length of 5, for example, it will be able to receive 5 items before blocking. Receiver will then be able to get this batch at once. 

```go
ch := make(chan int, 5)
```

#### Discarding values

A channel receiver can be used solely as a wait operation, since it blocks until receiving an item. The value will simply be discarded once ready and the code will proceed from there. 

```go
func waiting(r chan struct{}) {
    for {
        <-r
        fmt.Println("Ready!")
    }
}
```

#### Closing channels

Even though it is not needed to intentionally close a channel, we can do so by calling close:

```go
close(ch)
```

We can then check if a channel is closed, by using the `comma OK idiom`:

```go
value, ok := <-ch
if !ok {
    // 
}
```

Receiving from a nil channel (declared but uninitialized) will block the receiver forever.

Receiving from a closed channel simply returns the zero value of the channel type. 

While receiving from a closed channel is harmless, sending to it will cause trouble since passing data to a closed channel sender will cause a `panic`, meaning that the routine will crash. 

This normally wouldn't happen since the sender side logic is responsible for closing the channel or leaving it open, without passing more data to it, once it is done. 

#### Range

Channels can be used as iterables in for loops:

```go
for i := range ch {
    //
}
```
It will only iterate after a value is received and it will exit the loop once the channel is closed, on the sender side. 

Important to note that only `receiving` or `bidirectional` channels can be ranged this way.  

#### Select 

Similar to `switch` statements, applied to multiple channels. The case block with the first channel receiving a value, will execute.

If multiple channels are ready to receive a value at the same time, one will be executed randomly. 

```go
select {
case i, ok := <-chInts:
    if ok {
        // 
    }
case s, ok := <-chStr:
    if ok {
        // 
    }
}
```

## Mutexes

`Mutex` is a type that implements a protection or a `Lock` on given blocks of code, allowing for it run safely without the danger of incoming `race-conditions`, when multiple `goroutines` are executing. This is very important when `goroutines` are accessing or modifying data structures.

The good practice use in Go, is to wrap a block of code with a function, allowing the `mutex` to be `Unlocked` with a `defer`:


```go
var mu sync.Mutex

func() {
    mu.Lock()
    defer mu.Unlock()
    // ...

}()
```
The example shows an anonymous function executing in place with a `mutex` protection, but it is also common to create and define named functions to execute logic that needs protection, using this technique.

### RW Mutex

The `sync.RWMutex` is a standard library type that adds a specific read `lock` method called `RLock()`. 

`RLock()`, allows multiple `goroutines` to read from shared resources at the same time, but (this is crucial), only while there are no current active `Locks` (`writers`) in place in the current `mutex`.

In this example:

```go
var mu sync.RWMutex

func incFirst(nums []int) {
    if 
    mu.Lock()
    defer mu.Unlock()
    nums[0] += 1
}

func readFirst(nums []int) {
    mu.RLock()
    defer mu.RUnlock()
    fmt.Printf("%d\n", nums[0])
}
```

the `readFirst` function will only unlock for multiple reads in the `nums` slice when there is no current `Lock` in `incFirst`.  

## Generics

Allows the use of `type parameters`,  which are variables that refer to specified types, where the specified type could be any of the built-in types in Go, or a specific type for certain needs. 

If we need to build a function that does an operation on, lets say, a resource, and does not care what type that resource will be, we can use `generics` in order to avoid duplicating code and use the exact same function, regardless of the given type. 

```go
func readSlice[T any](someSlice T) {
    for v := range someSlice {
        fmt.Println(v)
    }
}
```

The `type parameter` here is set to `any`, which allows the function to receive a slice of any type.

### Constraints

With `generics`, we can also specify more information and scope concerning the `type parameter`. 

For example, if instead of declaring `[T any]` we do `[T colors]`, where `color` is a specific `interface`, we are informing that the type parameter for `T` will be a `concrete type` that implements that interface. This constraint only allows the function to receive values of the same `concrete type` and not a possible mix of different types, all implementing the same `interface`. 

Lets suppose we have 2 types that implement a `colors` interface: `Red` and `Blue`. When doing: 


```go

func paintObject[T colors](firstTone T, secondTone T) error {
    // ..... 
}
```

only a concrete type implementing the `colors` interface can be passed in, either `Red` or `Blue`. Never both.

```go
var redTone Red
var redTone2 Red

// allowed 
err := paintObject(redTone, reTone2)
```


```go
var redTone Red
var blueTone Blue

// not allowed
err := paintObject(redTone, blueTone)
```

### Interface Type Lists

In GO, interfaces can also be just a collection of concrete types:


```go
// Ordered matches any type that supports <, <=, >, and >=.
type Ordered interface {
    ~int | ~int8 | ~int16 | ~int32 | ~int64 |
        ~uint | ~uint8 | ~uint16 | ~uint32 | ~uint64 | ~uintptr |
        ~float32 | ~float64 |
        ~string
}

// Because T is constrained by Ordered, the compiler knows
// that < is valid for any T used with this function.
func Min[T Ordered](a, b T) T {
    if a < b {
        return a
    }
    return b
}
```
`~int`, means `int` or any named type whose underlying type is `int`. For example, when using type definitions like `type someInt int`. 

Like the previous example, only 1 concrete type is allowed. 



### Parametric Constraints


Interfaces can also use `type parameters`:

```go 
type store[P products] interface {
    Sell(P)
}
```

In this example, a type implements `store` not only if it has a `Sell()` method, but if this method takes the parameter of type 'P', where 'P' needs to satisfy the `product` interface constraint. 






