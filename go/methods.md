##Methods with pointer receivers

There are two reasons to use a pointer receiver. First, to avoid copying the value on each method call (more efficient if the value type is a large struct). Second, so that the method can modify the value that its receiver points to.

Methods裡面使用pointer有兩個原因
1. 避免複製副本
2. 這樣method才能夠修改呼叫者的value

```go
func (v *Vertex) update1() {
  v.X = 9
}

func (v Vertex) update2() {
  v.X = 9
}

func main() {
  v1 := &Vertex{3, 1}
  v2 := &Vertex{3, 1}
  fmt.Println("v1.X", v1.X)
  fmt.Println("v2.X", v2.X)
  v1.update1()
  v2.update2()

  fmt.Println("update")
  fmt.Println("v1.X", v1.X)
  fmt.Println("v2.X", v2.X)
}
```
