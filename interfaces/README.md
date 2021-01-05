#### Interfaces


```go
package main
import (
  "fmt"
)

type Shape interface {
  Area() float64
  Name() string
}

type triangle struct { 
  base float64 
  height float64 
} 

func (t triangle) Area() float64 {
  return (t.base * t.height) / 2
}

func (t triangle) Name() string {
  return "triangle"
}

type rectangle struct { 
  length float64 
  width float64 
}

func (r rectangle) Area() float64 {
  return r.length * r.width
}

func (r rectangle) Name() string {
  return "rectangle"
}

type square struct { 
  side float64 
}

func (s square) Area() float64 {
  return s.side * s.side
}

func (s square) Name() string {
  return "square"
}

// variadic parameter: accept zero or more arguments for `Shape` type
func printShapeDetails(shapes ...Shape) { 
  for _, item := range shapes { 
    fmt.Printf("The area of %s is: %.2f\n", item.Name(), item.Area()) 
  } 
} 

func main() { 
  t := triangle{base: 15.5, height: 20.1} 
  r := rectangle{length: 20, width: 10} 
  s := square{side: 10} 
  printShapeDetails(t, r, s)
} 
```

##### Empty Interface

```go
package main 
import "fmt"  

type person struct {
  firstName string
  age int
}

// parameter is an empty interface, it allows any types of parameters
func getType(data interface{}) string {
  switch data.(type) { 
    case int:  
      return "int" 
    case bool: 
      return "bool" 
    case string: 
      return "string" 
    case person: 
      return "person" 
    default:
      return "unknown"
  }
}

func main() {
  m := make(map[string]interface{}) 
  p := person{firstName: "Don", age: 34}
  m["person"] = p
  m["name"] = "Jorge"
  m["age"] = 31
  m["isMarried"] = false

  for _, v := range m {
    t := getType(v)
    fmt.Println(t)
  }
}
```