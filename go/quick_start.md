
##安裝Go
https://golang.org/doc/install

##設定環境變數
```sh
export PATH=$PATH:/usr/local/go/bin
export GOPATH=$HOME/projects/go
```

##Hello World
1. 建立 `GOPATH/src/github.com/your-github-name/hello/hello.go`

2. 貼上下面的code

```go
package main

import "fmt"

func main() {
    fmt.Printf("hello, world\n")
}
```

3. compile it

```
$ go install github.com/your-github-name/hello
```

4. Execute the command to see the greeting:

```
$ $GOPATH/bin/hello
hello, world
```
## name
講解 golnag 的命名慣例
i.e. public function 的第一個字元要大寫
使用 MixedCaps 而不使用 underscore (i.e. MyReader)


https://golang.org/doc/effective_go.html#names