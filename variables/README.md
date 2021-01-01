#### Variable Declaration
```go
/* declare a string type variable named `foo` */
var foo string = "bar"

/* declaring multiple variable with one var */
var ( 
  flag bool = false
  level string  = "info"
  startUpTime time.Time = time.Now()
) 

// we can skip type or value to get the default type or value
flag bool  // boolean's zero value is `false`
level = "info" // it has non-zero value to get the type

/* Wrong type error */
package main
import "math/rand"
func main() {
  var seed = 1234456789 // default type will be `int` based on the value
  rand.Seed(seed) // `rand.Seed` method requires a value of the `int64` type; Hence, it create an error
}

/* With correct type */
package main
import "math/rand"
func main() {
  var seed int64 = 1234456789
  rand.Seed(seed)
}

/* Short variable declaration using `:=` */
flag := false
level := "info"
startUpTime := time.Now()

/* Declare multiple variables using the same short declaration */
package main 
import ( 
  "fmt" 
  "time" 
) 

func main() { 
  flag, level, startUpTime := false, "info", time.Now() 
  fmt.Println(flag, level, startUpTime) 
}

/* Multiple variables using a function */
package main 
import ( 
  "fmt" 
  "time" 
)

func getConfig() (bool, string, time.Time) {
  return false, "info", time.Now()
}

func main() { 
  flag, level, startUpTime := getConfig() 
  fmt.Println(flag, level, startUpTime) 
}

// if we used `var` that would be like the following:
var ( 
  flag bool 
  level string 
  startUpTime time.Time 
)

flag, level, startUpTime = getConfig()

// change multiple values at once
a, b, c := "foo", 10, 0
a, b, c = "bar", c, 15
fmt.Println(a, b, c)  // bar 0 15
```


#### Variable types and their Zero values

| Type                     | Zero value    |
| ------------------------ | ------------- |
| bool                     | false         |
| Numbers (integers/float) | 0             |
| String                   | ""            |
| pointers, functions, interfaces, channels, slices, maps  | nil             |

#### Print format using `fmt.Printf`
| Substitution   | Formatting                        |
| -------------- | --------------------------------- |
| %v             | Any value without caring the type |
| %+v            | Print values with extra info      |
| %#v            | %+v with the addition of the name of the type |
| %T             | Print the variable type           |
| %d             | Decimal (base 10)                 |
| %s             | String                            |


```go
a := 10
fmt.Printf("a : %#v \n", a)
```

#### Pointer
```go
func main() { 
  var count1 *int
  count2 := new(int)
  countTemp := 5
  count3 := &countTemp

  if count1 != nil {
    // wont' be here; because the value is `nil`
    fmt.Printf("count1: %#v\n", *count1)
  }

  if count2 != nil {
    fmt.Printf("count2: %#v\n", *count2) // count2: 0
  }
  
  if count3 != nil {
    fmt.Printf("count3: %#v\n", *count3) // count3: 5
  }
}

```