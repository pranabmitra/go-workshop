#### Functions

##### Variadic Function
```go
// variadic variable can accept zero or more variables as the argument
func nums(i ...int) { 
  fmt.Println(i) 
}

nums(99,100) 
nums(200) 
nums()

/* To pass slice data as an arugment */
func main() {
  i := []int{5,10,15}
  // we have to unpack the slice data and then pass it
  nums(i...)
}

func nums(i ...int) {
  fmt.Println(i)
}

/* Full program: Get the Sum */
package main
import (
  "fmt"
)

func main() {  
  i := []int { 5, 10, 15 } 
  fmt.Println(sum(5, 8))  // 13
  fmt.Println(sum(i...)) // 30
}

func sum(nums ...int) int { 
  total := 0 
  for _, num := range nums { 
    total += num 
  } 

  return total 
}
```

##### Anonymous Function

```go
// IIFE
func main() {
  message := "Greeting"

  func(str string) {
    fmt.Println(str)
  }(message)
}

// assign the anonymous function to a variable
func main() {
  num := 9
  square := func(i int)int {
    return i * i
  }
  
  fmt.Printf("The square of %d is %d\n", num, square(num))
}
```

##### Closures
```go
func main() {
  increment := incrementor() 
  fmt.Println(increment()) // 1
  fmt.Println(increment()) // 2
  fmt.Println(increment()) // 3
}

func incrementor() func() int { 
  i := 0 

  return func() int { 
    i += 1 
    return i 
  } 
} 
```

##### Function Types

```go
// we can use function as a type
// `calc` is the type func and `(int int) string` is the signature (params + return type)
type calc func(int, int) string
```

##### Defer Function

```go
func main() {
  val := 10

  f1 := func() { 
    fmt.Println("Start") 
  } 
  
  f2 := func() { 
    fmt.Println("End") 
  } 

  defer func () {
    val += 15;
    fmt.Printf("value inside the defer method: %d.\n", val)
  }() 

  f1();
  f2();
  fmt.Printf("value is %d.\n", val)
}

/* --- output: ---
  Start
  End
  value is 10.
  value inside the defer method: 25.
*/
```


##### Errors

```go
/* Custom errors */
package main
import (
  "fmt"
  "errors"
)

func main() {
  // we're creating two custom errors
  var (
    ErrName = errors.New("invalid name")
    ErrNumber = errors.New("invalid number") 
  )

  fmt.Println(ErrName)
  fmt.Println(ErrNumber)
}

/* Panic errors */
nums := []int{2, 4, 6, 8} 
fmt.Println(nums[4]) 
/*
  This will throw an error like below:
  `panic: runtime error: index out of range [4] with length 4` 
 */

```

> Panic is a built-in function that causes the program to crash. It stops the normal execution of the Goroutine.

```go
/* Creating a panic */
package main 
import ( 
  "errors" 
  "fmt" 
) 

func main() { 
  msg := "bye" 
  message(msg) 
  fmt.Println("This line will not get printed") 
} 

func message(msg string) { 
  if msg == "bye" { 
    panic(errors.New("something went wrong")) 
  } 
} 

// However, if we use defer function, that will be executed 
func message(msg string) { 
  f := func() { 
    fmt.Println("Defer in message func") 
  } 
  
  defer f()

  if msg == "bye" { 
    panic(errors.New("something went wrong")) 
  } 
} 
// output will be:
Defer in message func
panic: something went wrong
goroutine 1 [running]:
... // other error info
```

> Recover `recover()` is a function that is used to regain control of a panicking Goroutine. The `recover()` function is only useful inside a deferred function.

```go
/* Recovering from a panic */
package main 
import ( 
  "errors" 
  "fmt" 
) 

var ErrInfo = errors.New("invalid information")

func main() { 
  msg := "bye" 
  message(msg) 
  // This line will be executed as we've recovered from the panic
  fmt.Println("This line will not get printed") 
} 

func message(msg string) { 
  f := func() { 
    if r := recover(); r != nil {
      if r == ErrInfo { 
        fmt.Printf("An error has occured, err: %v\n", r) 
      }
    }
  } 

  defer f()

  if msg == "bye" { 
    panic(ErrInfo)
  } 
} 
// output:
An error has occured, err: invalid information
This line will not get printed
```