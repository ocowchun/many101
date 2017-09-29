https://blog.golang.org/defer-panic-and-recover

Defer statements allow us to think about closing each file right after opening it, guaranteeing that, regardless of the number of return statements in the function, the files will be closed.

The behavior of defer statements is straightforward and predictable. There are three simple rules:

1. A deferred function's arguments are evaluated when the defer statement is evaluated.

```go
func a() {
    i := 0
    defer fmt.Println(i)
    i++
    return
}
```

2. Deferred function calls are executed in Last In First Out order after the surrounding function returns.

This function prints "3210":

```go
func b() {
    for i := 0; i < 4; i++ {
        defer fmt.Print(i)
    }
}
```

3. Deferred functions may read and assign to the returning function's named return values.

In this example, a deferred function increments the return value i after the surrounding function returns. Thus, this function returns 2:

```go
func c() (i int) {
    defer func() { i++ }()
    return 1
}
```