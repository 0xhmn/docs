### web servers
- using pakcage `net/http`

- simple hello world (just like node server
- why we are passing `http.Request` by pointer? because we are avoiding copying of a large struct. It's more efficient
```
package main
import (
   "net/http"
)
func main() {
   http.HandleFunc("/", func(w http.ResponseWriter, req *http.Request) {
      w.Write([]byte("this is a test"))
   })   
   http.ListenAndServe(":9000", nil)
}
```
- or we can not using the default server (by passing `nil`) and create our own server
```
package main
import (
    "fmt"
    "log"
    "net/http"
)
type Hello struct {}
func (h Hello) ServeHTTP (w http.ResponseWriter, r *http.Request) {
    fmt.Fprint(w, "Hello!")
}
func main() {
    var h Hello
    err := http.ListenAndServe("localhost:4000", h)
    if err != nil {
        log.Fatal(err)
    }
}
```