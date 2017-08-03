https://tour.golang.org/methods/9

pending at https://tour.golang.org/methods/12

https://research.swtch.com/interfaces

## The empty interface
The interface type that specifies zero methods is known as the empty interface:

```go
interface{}
```

An empty interface may hold values of any type. (Every type implements at least zero methods.)

Empty interfaces are used by code that handles values of unknown type. For example, `fmt.Print` takes any number of arguments of type `interface{}`.