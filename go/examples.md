## Basic Examples

### Making a random number

```go
package main

import (
    "fmt"
    "math/rand"
    "time"
)

func main() {
    
    // the value will be the same all the time
    fmt.Println("My favorite number is ", rand.Intn(100))
    
    // using a rand source
    s1 := rand.NewSource(time.Now().UnixNano())
    r1 := rand.New(s1)
    
    fmt.Print(r1.Intn(20))
}
```

### Making a 3x3 matrix and print it using strings.join
```go
package main
import (
    "fmt"
    "strings"
)

func main() {
    // making a matrix of 3x3
    game := [][]string{
        []string{"-", "-", "-"},
        []string{"-", "-", "-"},
        []string{"-", "-", "-"},
    }
    
    // changing some values
    game[0][0] = "O"
    game[2][2] = "X"
    game[1][1] = "M"    
    
    fmt.Println(game)
    fmt.Println(len(game),"\n")
    
    printBoard(game)
}
```

### Another example of making a matrix of nXm
```go
func Pic(dx, dy int) [][]uint8 {
    img := make([][]uint8, dy)
    // |
    // |
    // dy
    // |
    // |
    
    for i:= 0; i < dy; i++ {
        // making a slice of lenght dx (dx columns) in each row
        img[i] = make([]uint8, dx)
        for j :=0; j < dx; j++ {
            img[i][j] = uint8(i^j+(i+j)/2)
        }
    }
    return img
}
```

### Counting the number of words using a map
```go
package main

import (
    "strings"
    "fmt"
)

func main() {
    fmt.Println(WordCount("test this is a test"))
}

func WordCount(s string) map[string]int {
    // make a new map
    m := make(map[string]int)
    arr := strings.Fields(s)
    
    for i:= 0; i < len(arr); i++ {
        m[arr[i]]++
    }

    return m  
}
```