### Pointers
- basic
    - we get the memory address using `&`
```
package main
import "fmt"
const meterToYard float64 = 1.09
func main() {
    var meters float64
    fmt.Println("Enter meters swam: ")
    // put the value from the user on "meters" memory place
    fmt.Scan(&meters)
    yards := meters*meterToYard
    fmt.Println("It's ", yards, " yards!")
}
```
    - we point to a memeory address using `*`
```
package main
import (
    "fmt" 
    "reflect"
)
const meterToYard float64 = 1.09
func main() {
    a := 34
    fmt.Println(a)
    fmt.Println(&a)
    var b *int = &a                 //is equal to var b = &a
    fmt.Println(b)                  // will print address of a
    fmt.Println(*b)                 // 34
    fmt.Println(reflect.TypeOf(b))  // *int
}
```
- pointer as a function argument
```go
package main
func main() {
   var i int = 2
   var p *int = &i
   f(p)              // or we can say f(&i)
   println(i)        // 5
}
// writing the value of 5 to the location specified by the pointer passed to this function
func f(ip *int) {
    *ip = 5
}
```

- pointer to pointer // https://www.eskimo.com/~scs/cclass/int/sx8.html
```
package main
func main() {
   var i, j, k int = 5,6,7
   // ip1 to i and ip2 to j
   var ip1 *int = &i
   var ip2 *int = &j  
   // ipp a pointer to pointer
   var ipp **int = &ip1     // or ipp := &ip1  
   // now, the pointer pointed to by ipp (which is ip1) points to ip2
   // it means println(*ip2) is 6
   *ipp = ip2  
   // now, the pointer pointed to by ipp (ip1 again) will points to k
   // *ip2 is 7
   *ipp = &k
   println(*ip1)
}
```
        
- more example
```
func main() {
    i, j := 42, 2834
    
    p := &i     // point to i
    j++
    
    // p and i pointing to the same place (i.e. 0xc0820062b0)
    fmt.Println(&i)
    fmt.Println(p)
    
    // changed the value of pointer -> i is changed
    *p = 7
    fmt.Println(&i)
    fmt.Println(i)
    
    // pointing to j
    p = &j
    *p = *p / 37
    fmt.Println(j)
    fmt.Println(*p)
}
```
The type *T is a pointer to a T value. Its zero value is nil.
```
var p *int
// for instantiating:
i := 13
var pp *int = &i
```

### Example
- [this question](http://stackoverflow.com/questions/34521255/getting-different-values-by-passing-pointers-to-a-function-in-go)
- passing a pointer to a funtion in three different ways:

1. this will print 2 (doesn't change the value), *because the pointer in the function is just a local pointer and by changing it, we are not changing the value it points to*
```
package main
type Test struct { Value int}
func main() {
   var i Test = Test {2}
   var p *Test = &i
   println(p)
   f(p)
   println(p)        // same location as p before function
   println(i.Value)  // 4
}
func f(p *Test) {
   p = &Test{4}
   println("local p", p)   // different p location (this p is local in the function)
}
```
2. but this one print 4 (it doest change the value) *because we are changing the value that the pointer is pointing to*
```
package main
type Test struct { Value int}
func main() {
   var i Test = Test {2}
   var p *Test = &i
   f(p)
   println(i.Value)  // 4
}
func f(p *Test) {
   *p = Test{4}
}
```
3. and another way to get a 4
```
package main
type Test struct { Value int}
func main() {
   var i Test = Test {2}
   var p *Test = &i
   f(p)
   println(i.Value)  // 4
}
func f(p *Test) {
   p.Value = 4       // it's equal to (*p).Value = 4
}
```
5. And if we want to change the value of local pointer `p` in the main function, we need to use pointer to pointer
```
package main
type Test struct { Value int}
func main() {
   var i Test = Test {2}
   var p *Test = &i        // local pointer p in main function
   f(&p)
   println(i.Value)        // 2
   println(p.Value)        // 4
}
func f(pp **Test) {
   *pp = &Test{4}           // 
}
```