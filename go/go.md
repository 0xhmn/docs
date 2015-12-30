# Table of Contents
1. [Generl Tips](#id-section1)
2. [Basic Examples](./examples.md)

<div id='id-section1'/>
## Generl Tips


### operators
- The `:=` notation serves both as a declaration and as initialization.
[source](http://stackoverflow.com/questions/16521472/assignment-operator-in-go-language)
```
foo := "bar"
is equivalent to
var foo = "bar"
or
var foo string = "bar"
```
```
name := "John"
is just syntactic sugar for
var name string
name = "John"
```
**NOTE THAT `r:=1` only works in a function, not the package level**


- In Go, a name is exported if it begins with a capital letter.
Foo is an exported name, as is FOO. The namefoo is not exported.
i.e.:
you can't get an outcome from
```
func main() {
	fmt.Println(math.pi)
}
```
but 
```
func main() {
	fmt.Println(math.Pi)
}
```

- in function types come AFTER the var name:
```
func add(x int, y int ) int {
    return x+y
}
```
or 
```
func add(x, y int) int {
    return x+y
}
```

- IMPORTANT: a function can return any number of results
```
package main
import "fmt"
func swap(x, y string) (string, string) {
	return y, x
}
func main() {
	a, b := swap("hello", "world")
	fmt.Println(a, b)
}
```

- IMPORTANT: naked returning
```
func split(sum int) (x, y int) {
    x = sum * 2
    y = sum / 2
    // a naked return - will return named return values
    return
}
func main() {
    c, d := split(10)
    fmt.Println("c is equal to ", c , "and d is ", d)    
}
```

- var keyword in GO
it's used to declare a list of variables
```
package main
import "fmt"
var c, python, java bool
// no need to mention the type if we initialize the vars
var e,f,k = true, 23, "test"
// also we can define all of them in one block
var (
	ToBe   bool       = false
	MaxInt uint64     = 1<<64 - 1
	z      complex128 = cmplx.Sqrt(-5 + 12i)
)
func main() {
	var i int
	fmt.Println(i, c, python, java)   // 0 false false false
}
```





### Types
- Go's basic types are
```
bool
string
int  int8  int16  int32  int64
uint uint8 uint16 uint32 uint64 uintptr
byte // alias for uint8
rune // alias for int32
     // represents a Unicode code point
float32 float64
complex64 complex128
```
initiating methods:
```
var i int = 13
i := 13
```

- get the type using printf
```
package main
import "fmt"
func main() {
	v := 42.2 // change me!
	fmt.Printf("v is of type %T\n", v)
}
```
- defining constant with const keyword
```
const Pi = 3.141592653589793 
```

### basic functions
- for loops
```
for i:=0; i<10; i++ {
    fmt.Println(i)
}
```
- while loops
there is no while loop! Go uses 'for' instead
```
i := 1
for i < 20 {
    fmt.Println(i)
    i++
}
```
- Infinite loop
```
for {
}
```
- if
```
if x < 0 {
}
```
we can add a statement before if condition
```
func ifTest(x , n, lim float64) float64 {
    if v:= math.Pow(x, n); v > lim {
        return lim
    }
        return 0
}
```
- Switch statement
Like if you also can have a statement before switched value:
```
switch os := runtime.GOOS; os {
case "darwin":
	fmt.Println("OS X.")
case "linux":
	fmt.Println("Linux.")
default:
	// freebsd, openbsd,
	// plan9, windows...
	fmt.Printf("%s.", os)
}
```
Or have one or no statement:
```
t := time.Now()
switch {
case t.Hour() < 12:
	fmt.Println("Good morning!")
case t.Hour() < 17:
	fmt.Println("Good afternoon.")
default:
	fmt.Println("Good evening.")
}
```
- defer keyword
The deferred call's arguments are evaluated immediately, but the function call is not executed until the surrounding function returns.
Deferred function calls are pushed onto a stack. When a function returns, its deferred calls are executed in last-in-first-out order.
```
func main() {
	fmt.Println("counting")

	for i := 0; i < 10; i++ {
		defer fmt.Println(i)
	}

	fmt.Println("done")
}
```
Will return
```
counting
done
9
8
7
6
5
4
3
```

### Struct
- basic
```
type Vertex struct {
    X int
    Y int
}
// getter
func getXfromVertex(v Vertex) int {
    return v.X
}
func main() {
    v := Vertex{1,3}
    fmt.Println(v)
    v.X = 4
    fmt.Println(v)
    
    fmt.Println(getXfromVertex(v))
}
```

- we can instantiate a struct in three different ways

	1- using `var VarName StructTupe`:
    ```
	type courseMeta struct {
	    Author string
	    Level int
	    Rating float32
	}
	func main() {   
	    var DockerDeepDiv courseMeta
	    
	    DockerDeepDiv.Author = "author test"
	    DockerDeepDiv.Level = 2
	    DockerDeepDiv.Rating = 3.4
	    fmt.Println(DockerDeepDiv)
	}
	```
    
	2- using `:= type{//initialize or keep blank}`
    ```
	func main() {   
	    DockerDeepDiv := courseMeta{}
	    DockerDeepDiv.Author = "test"
	    fmt.Println(DockerDeepDiv)
	}
	```
    
	3- using `NewName := new(structType)`
    ```
	// the new keyword will give us a pointer
	func main() {   
	    DockerDeepDiv := new(courseMeta)
	    fmt.Println(DockerDeepDiv)
	}
    ```

- The biggest difference with a java class:
Partial code: In Java 
```
class House {
    public String getHouseName() {  //method defined within class
        //implementation
    }
}
```
Partial code: In Go
```
type House struct { }
func (h House) GetHouseName() string { } //method defined outside of struct, but works on House
```
[source](http://golangtutorials.blogspot.com/2011/06/structs-in-go-instead-of-classes-in.html)

### Arrays
- Like type, the capacity comes BEFORE the type
```
var a [10]int
func main() {
	var a [2]string
	a[0] = "Hello"
	a[1] = "World"
	fmt.Println(a[0], a[1])
	fmt.Println(a)
	
	    // or declare array like this
	// note that you cant use the syntax like s:=[23]int
	    s:= []int {12,23}
}
```
- To get the length of the array
```
Len(a)
```
- Like python we can slice the array:
```
S:=[]int{1,3,4,5,34,5,5}
S[2:5] // or S[3:]
```
- Making slices:
```
package main
import (
    "fmt"
)
func main() {
    s:= []int{1,2,4,4,53,4,34,23,4}
    PrintSlice("original S", s)
    
    a:= make([]int, 5)
    PrintSlice("a", a)
    
    b:= make([]int, 0, 5)
    PrintSlice("b", b)
    
    c:= b[:2]
    PrintSlice("c", c)  // length: 2, cap: 5
    
    d:=c[2:5]
    PrintSlice("d", d)
}
// PrintSlice will print something
func PrintSlice(s string, x []int) {
    fmt.Printf("%s len: %d cap: %d %v\n", s, len(x), cap(x), x)
}
// original S len: 9 cap: 9 [1 2 4 4 53 4 34 23 4]
// a len: 5 cap: 5 [0 0 0 0 0]
// b len: 0 cap: 5 []
// c len: 2 cap: 5 [0 0]
// d len: 3 cap: 3 [0 0 0]
```

- Nil slices
The zero value of a slice is nil.
A nil slice has a length and capacity of 0. From <https://tour.golang.org/moretypes/11> 

- Adding element to a slice.
Using `append(slice, elements)`
It will return `[]Type`

- Range
It's used to do a for loop for slices
It returns two values, first the index and then the value of element in the array
```
var pow = []int{1, 2, 4, 8, 16, 32, 64, 128}
func main() {
	for i, v := range pow {
		fmt.Printf("2**%d = %d\n", i, v)
	}
}
```
You can skip the index or value by assigning to _.
If you only want the index, drop the ", value" entirely.

### Map
- basic
```
map[KeyType]ValueType
```
- NOTE:
Maps must be created with make before use; the nil map is empty and cannot be assigned to.
Example: 
```
type Vertex struct {
    Lat, Long float64
}
// defining a map
// key: a string
// value : Vertex
var m map[string]Vertex
func main() {
    // making the map
    m = make(map[string]Vertex)
    m["Bell Labs"] = Vertex{40.6, -74.39}
    fmt.Println(m["Bell Labs"])
}
```
- Or we simply can say
```
var m = map[string]Vertex{
	"Bell Labs": Vertex{
		40.68433, -74.39967,
	},
	"Google": Vertex{
		37.42202, -122.08408,
	},
}
```
Or even shorter!
```
var m = map[string]Vertex{
	"Bell Labs": {40.68433, -74.39967},
	"Google":    {37.42202, -122.08408},
}
```
- Map Operations
```
m := make(map[string]int)
m["One"] = 1
m["Two"] = 2
// update one
m["One"] = 3
// remove two
delete(m, "Two")
```

### func as argument
- passing func as an argument
```
func compute(fn func(float64, float64) float64) float64 {
	return fn(3, 4)
}
```
- go closures
closure is a function value that references variables from outside its body
```
// adder will return a func that gets int and returns int
func adder() func(int) int {
    sum := 0
    return func(x int) int {
        sum += x
        return sum
    }
}
func main() {   
    pos, neg := adder(), adder()
    fmt.Println(pos(2)) // 2
    fmt.Println(pos(3)) // 5

    fmt.Println(neg(-5)) // -5
    fmt.Println(neg(-1)) // -6
}
```
- // NOTE: unlike javascript you cannot print the function (pos or neg)

### Methods
- **IMPORTANT: **
we can define methods on struct types instead of classes
In Go, you define a method receiver to specify which struct to attach a certain function to in order to make it invoke-able as a method.
```
package main
import "fmt"
type Dog struct {
}
func (d Dog) Say() {
    fmt.Println("Woof!")
}
func main() {
    // NOTE:
    // d is a pointer that points to Dog{}
    // it's equal to
    // var d *Dog = &Dog{}
    d := &Dog{}
    d.Say()
}
```
- why we need to use pointer to pass the new dog:[HERE](http://nathanleclaire.com/blog/2014/08/09/dont-get-bitten-by-pointer-vs-non-pointer-method-receivers-in-golang/)
we may want to pass the method's value by reference (using pointer) and not by value (default everywhere)
```
package main
import (
    "fmt"
)
type Mutable struct {
    a int
    b int
}
func (m Mutable) StayTheSame() {
    m.a = 3
    m.b = 13
}
func (m *Mutable) Mutatue() {
    m.a = 3
    m.b = 13
}
func main() {
    // assign
    m := &Mutable{0, 0}
    m.StayTheSame()
    fmt.Println(m)
    m.Mutatue()
    fmt.Println(m)
    fmt.Print(m.a)
    // OOP WAY
    m.a = 12212
    fmt.Println(m.a)
}
// λ go run hello.go
// &{0 0}
// &{3 13}
// 312212
```

- we can define methods on any type (not just structs)
    - **it's like CSHARP extention method**
- note that this time we didn't use pointer
```
package main
import (
	"fmt"
	"math"
)
type MyFloat float64
func (f MyFloat) Abs() float64 {
	if f < 0 {
		return float64(-f)
	}
	return float64(f)
}
func main() {
	f := MyFloat(-math.Sqrt2)
	fmt.Println(f.Abs())
}
```
- Another example
```
package main
import (
    "fmt"
)
type Vertex struct {
    X, Y float64
}
func (v *Vertex) Scale(f float64) {
    v.X = v.X*f
    v.Y = v.Y*f
}
func (v Vertex) Abs() float64{
    return v.X*v.X + v.Y*v.Y
}
func main() {
    v := &Vertex{3,4}
    fmt.Println(v.Abs())
    v.Scale(2.1)
    fmt.Println(v.Abs())
}
```

### Interfaces
- it's a set of methods:
```
type Abser interface {
    Abs() float64
}
```
- *didn't get this example completely*
```
package main
import (
    "fmt"
)
// Interface
type Abser interface {
    Abs() float64
}
type MyFloat float64
func (f MyFloat) Abs() float64 {
    if f < 0 { return float64(-f) }
    return float64(-f)
}
type Vertex struct {
    X, Y float64
}
func (v *Vertex) Abs() float64 {
    return v.X*v.Y  // just for the test
}
func main() {
    var a Abser
    f := MyFloat(-23.232)
    v := Vertex{2.4, 6.2} 
    a = f   // a MyFloat implements Abser (IT IS NOT F=A)
            // ALSO if we want to implement an interface we need to have that method
    fmt.Println(a.Abs())   
    // a = v  // YOU'LL GET AN ERROR
    a = &v  // a *Vertex implements Abser
    fmt.Println(a.Abs())
}
```
- another example
```
package main
import (
    "fmt" 
)
type Drawer interface {
    Draw()
}
type Circle struct {
    r int
}
func (c Circle) Draw() {
    fmt.Println("draw a circle")
}
type Rectangular struct {
    a,b int
}
func (r Rectangular) Draw() {
    fmt.Println("Draw a rectangular")
}
func DrawForm(d Drawer) {
    d.Draw()
}
func main() {
    c := Circle{2}
    r := Rectangular{2,4}
    // var d Drawer
    // d = c
    // d = r
    // fmt.Println(d)  // {2. 4}
    // d.Draw()
    DrawForm(c)
    DrawForm(r)
}
// λ go run hello.go
// draw a circle
// Draw a rectangular
```
- overriding
here we are overriding the fmt (to string) function - note that we're using `String` and not string
```
package main
import (
    "fmt" 
)
type Person struct {
    name string
    age int
}
// https://golang.org/pkg/fmt/#Stringer
// type Stringer interface {
//         String() string
// } 
func (p Person) String() string {
    return fmt.Sprintf("%v (%v years)", p.name, p.age)
}
func main() {
    a := Person{"Hooman", 25}
    fmt.Println(a)
}
```

### Errors
- Go programs express error state with error values.
- The error type is a built-in interface similar to `fmt.Stringer`:
```
type error interface {
    Error() string
}
```
- example:
```
package main
import (
    "fmt"
    "time"
)
type MyError struct {
    ErrorTime time.Time
    ErrorType string
}
// overriding the Error
func (e MyError) Error() string {
    return fmt.Sprintf("at %v, %s happened!", e.ErrorTime, e.ErrorType)
}
func makeError() error{
    return MyError{time.Now(), "StackOverFlow"}
}
func main() {
    if err := makeError(); err != nil {
        fmt.Println(err)
    }
}
```