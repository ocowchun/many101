## pointers
Go has pointers. A pointer holds the memory address of a value.

The type `*T` is a pointer to a `T` value. Its zero value is nil.

```go
var p *int
```

The `&` operator generates a pointer to its operand.

```go
i := 42
p = &i
```

The `*` operator denotes the pointer's underlying value.

```go
fmt.Println(*p) // read i through the pointer p
*p = 21         // set i through the pointer p
```

This is known as "dereferencing" or "indirecting".

Unlike C, Go has no pointer arithmetic.

```go
package main

import "fmt"

func main() {
  i, j := 42, 2701

  p := &i         // point to i
  fmt.Println(p)  // read i through the pointer
  fmt.Println(*p) // read i through the pointer
  *p = 21         // set i through the pointer
  fmt.Println(i)  // see the new value of i

  p = &j         // point to j
  *p = *p / 37   // divide j through the pointer
  fmt.Println(j) // see the new value of j
}
```

`&variable` 會回傳 variable 的 memory address
`*pointer` 會回傳 pointer 對應的 memory address 的值, 或是可以修改他
就像是打開一個盒子 你可以看盒子裡面有什麼東西,又或是放東西進去盒子裡面。

## Pointers to structs
Struct fields can be accessed through a struct pointer.

To access the field X of a struct when we have the struct pointer p we could write (*p).X. However, that notation is cumbersome, so the language permits us instead to write just p.X, without the explicit dereference.


### when to use &struct?
You mainly want to use pointers when you're passing a variable around that you need to modify (but be careful of data races use mutexes or channels If you need to read and write to a variable from different functions)
https://stackoverflow.com/questions/34543430/golang-basics-struct-and-new-keyword

When passing a pointer, you can modify the original value. When passing a non pointer you can't modify it. That is because in go variables are passed as a copy. So in the iDontTakeAPointer function it is receiving a copy of the tester struct then modifying the name field and then returning, which does nothing for us as it is modifying the copy and not the original.


## Pointer receivers
You can declare methods with pointer receivers.

This means the receiver type has the literal syntax *T for some type T. (Also, T cannot itself be a pointer such as *int.)

For example, the Scale method here is defined on *Vertex.


`Methods with pointer receivers can modify the value to which the receiver points (as Scale does here).` Since methods often need to modify their receiver, pointer receivers are more common than value receivers.

Try removing the * from the declaration of the Scale function on line 16 and observe how the program's behavior changes.

With a value receiver, the Scale method operates on a copy of the original Vertex value. (This is the same behavior as for any other function argument.) The Scale method must have a pointer receiver to change the Vertex value declared in the main function.

```go
package main

import (
  "fmt"
  "math"
)

type Vertex struct {
  X, Y float64
}

func (v Vertex) Abs() float64 {
  return math.Sqrt(v.X*v.X + v.Y*v.Y)
}

func (v *Vertex) Scale(f float64) {
  v.X = v.X * f
  v.Y = v.Y * f
}

func main() {
  v := Vertex{3, 4}
  v.Scale(10)
  fmt.Println(v.Abs())
}
```


## Methods and pointer indirection
Comparing the previous two programs, you might notice that functions with a pointer argument must take a pointer:

```go
var v Vertex
ScaleFunc(v)  // Compile error!
ScaleFunc(&v) // OK
```
while methods with pointer receivers take either a value or a pointer as the receiver when they are called:

```go
var v Vertex
v.Scale(5)  // OK
p := &v
p.Scale(10) // OK
```

For the statement v.Scale(5), even though v is a value and not a pointer, the method with the pointer receiver is called automatically. That is, as a convenience, Go interprets the statement `v.Scale(5)` as `(&v).Scale(5)` since the Scale method has a pointer receiver.

```go
package main

import "fmt"

type Vertex struct {
  X, Y float64
}

func (v *Vertex) Scale(f float64) {
  v.X = v.X * f
  v.Y = v.Y * f
}

func ScaleFunc(v *Vertex, f float64) {
  v.X = v.X * f
  v.Y = v.Y * f
}

func main() {
    v := Vertex{3, 4}
  v.Scale(2)
  ScaleFunc(&v, 10)

  p := &Vertex{4, 3}
  p.Scale(3)
  ScaleFunc(p, 8)

  fmt.Println(v, p)
}
```

##Methods and pointer indirection (2)
The equivalent thing happens in the reverse direction.

Functions that take a value argument must take a value of that specific type:

```go
var v Vertex
fmt.Println(AbsFunc(v))  // OK
fmt.Println(AbsFunc(&v)) // Compile error!
```

while methods with value receivers take either a value or a pointer as the receiver when they are called:

```go
var v Vertex
fmt.Println(v.Abs()) // OK
p := &v
fmt.Println(p.Abs()) // OK
```

In this case, the method call `p.Abs()` is interpreted as `(*p).Abs()`.

```go
package main

import (
  "fmt"
  "math"
)

type Vertex struct {
  X, Y float64
}

func (v Vertex) Abs() float64 {
  return math.Sqrt(v.X*v.X + v.Y*v.Y)
}

func AbsFunc(v Vertex) float64 {
  return math.Sqrt(v.X*v.X + v.Y*v.Y)
}

func main() {
  v := Vertex{3, 4}
  fmt.Println(v.Abs())
  fmt.Println(AbsFunc(v))

  p := &Vertex{4, 3}
  fmt.Println(p.Abs())
  fmt.Println(AbsFunc(*p))
}
```

## Mehtod, Function and pointer
### Mehtod

```go
type Vertex struct {
  X, Y float64
}

func (v *Vertex) func1() {
  v.X = 10
}

func (v Vertex) func2(){
  return v.X + v.Y
}

func foo(){
  v := Vertex{3, 4}
  p := &Vertex{4, 3}
  v.func1() // ok
  p.func1() //ok
  v.func2() // ok
  p.func2() //ok

}

```

宣告 Mehtod 時使用 pointer receiver 的時候(i.e. `func1`),就算 `v` 是 value, Go interprets 會自動幫忙轉成 `(&v).func1()`

宣告 Mehtod 時使用 value receiver 的時候(i.e. `func2`),就算 `p` 是 pointer, Go interprets 會自動幫忙轉成 `(*p).func2()`

### Function

```go
type Vertex struct {
  X, Y float64
}

func func3(v *Vertex) {
  v.X = 10
}

func func4(v Vertex){
  return v.X + v.Y
}

func foo(){
  v := Vertex{3, 4}
  p := &Vertex{4, 3}
  func3(v) // Compile error!
  func3(p) //ok
  func4(v) //ok
  func4(p) // Compile error!

}

```

宣告 Function 時, 參數使用 value, 就一定要傳 value, 使用 pointer 就一定要傳 pointer