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

### [INHERITENCE EXAMPLE](http://thenewstack.io/understanding-golang-type-system/)

```go
package main

import (
  "fmt"
  "time"
)

//People interface
type People interface {
  SayHello()
  GetDetails()
}

type Person struct {
  name string
  age int
  city, phone string
}

// we are not using (p *Person) since we are not changing the values
func (p Person) GetDetails() {
  fmt.Printf("Name: %s, Age: %d, City: %s, Phone: %s\n", p.name, p.age, p.city, p.phone)
}

func (p Person) SayHello() {
  fmt.Printf("Hi, I am %s from %s\n", p.name, p.city)
}

type Speaker struct {
  // IMPORTANT: When we embed a type into another type, 
  // the methods of the embedded type will avail to the new type
  Person    // type embeding for composition
  speaksOn []string
  pastEvents []string
}

// override GetDetail for Speaker
func (s Speaker) GetDetails() {
  fmt.Printf("Speaker Detail: Name: %s, City: %s and will talk on following technologies: \n", s.name, s.city)
  for _, v := range s.speaksOn {
    fmt.Println("\t", v)
  }
}

type Organizer struct {
	Person //type embedding for composition
	meetups []string
}
// todo: override Organizer GetDetail

type Attendee struct {
  Person //type embedding for composition
  interests []string
}

/*
  1- EVERYTHING than has GetDetails() and SayHello() is considered as People
  2- Person has both of them
  3- Speaker, Organizer and Attendee embedded Person, so they also have GetDetail and SayHello
*/
type Meetup struct {
  location string
  city string
  date time.Time
  people []People
}

func (m Meetup) MeetupPeople() {
  for _, v := range m.people {
    v.GetDetails()
  }
}

func main() {
	shiju := Speaker{Person{"Shiju", 35,"Kochi" ,"+91-94003372xx"},
		[]string{"Go","Docker", "Azure","AWS"},
		[]string{"FOSS","JSFOO","MS TechDays"}}
	satish := Organizer{Person{"Satish", 35,"Pune" ,"+91-94003372xx"},
		[]string{"Gophercon","RubyConf"}}
	alex := Attendee{Person{"Alex", 22,"Bangalore" ,"+91-94003672xx"},
		[]string{"Go","Ruby"}}
    
    m := Meetup {"UMN","Saint Paul", time.Date(2015, time.February, 19, 9, 0, 0, 0, time.UTC), []People{shiju, satish, alex} }
    m.MeetupPeople()
}
```