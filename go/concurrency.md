# Goroutines
> A goroutine is a lightweight thread managed by the Go runtime.


https://tour.golang.org/concurrency/1

https://golangbot.com/concurrency/
https://golangbot.com/goroutines/
https://golangbot.com/channels/


```go
package main

import (
  "fmt"
)

func hello(done chan bool) {
  fmt.Println("Hello World")
  done <- true
}

func main() {
  done := make(chan bool)
  go hello(done)
  // block unitl some goroutine write data to `done`
  <-done
  fmt.Println("hello")
}
```

https://blog.golang.org/pipelines
https://stackoverflow.com/questions/20993139/how-does-goroutines-behave-on-a-multi-core-processor