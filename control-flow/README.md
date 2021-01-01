#### Logic and Loop

```go
// if-elseif-else
input := -10

if input < 0 {
  fmt.Println("input can't be a negative number")
} else if input%2 == 0 {
  fmt.Println(input, "is even")
} else {
  fmt.Println(input, "is odd")
}

/* Initial if */
// here we're validating the input first, then using it as a condition
if err := validate(input); err != nil {
  fmt.Println(err)
}

/* Switch statement */
package main 
import ( 
  "fmt" 
  "time" 
)

func main() { 
  day := time.Sunday

  switch day {
    // we can use multiple case values like this
    case time.Monday, time.Tuesday, time.Wednesday, time.Thursday, time.Friday:
      fmt.Println("This is a weekday")
    case time.Saturday, time.Sunday:
      fmt.Println("This is weekend")
    default:
      fmt.Println("Error, day is not valid")
  }
}

// we can use condition like the following:
case day == time.Sunday || day == time.Saturday:
fmt.Println("This is weekend")

/* Loops */
names := []string{"Jorge", "Jane", "Joe", "June"}
for i := 0; i < len(names); i++ {
  fmt.Println(names[i])
}


// Loop over map
config := map[string]string{
  "count":  "1",
  "level": "warn",
  "version": "1.0.0",
}

for key, value := range config {
  fmt.Println(key, "=", value)
}
/* output:
count = 1
level = warn
version = 1.0.0
*/

// Loop without condition
i := 1
for {
  if i > 5 {
    break
  }
  fmt.Println(i) // will print 1-5
  i++
}
```