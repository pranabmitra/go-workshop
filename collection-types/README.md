#### Complex/Collection Types


##### Array 

> Fixed-length ordered list
```go
/* Array of size 10 that contains ints */
var arr [10]int
fmt.Printf("%#v\n", arr)
// [10]int{0, 0, 0, 0, 0, 0, 0, 0, 0, 0}

/* Below three declarations wil generate the same output: [5]int{0, 0, 0, 0, 0} */
var arr1 [5]int
arr2 := [5]int{0}
arr3 := [...]int{0, 0, 0, 0, 0}
fmt.Printf("%#v\n%#v\n%#v\n", arr1, arr2, arr3)

/* Intialize an array using keys */
arr1 := [...]int{9: 0}
fmt.Printf("%#v\n", arr1)
// [10]int{0, 0, 0, 0, 0, 0, 0, 0, 0, 0}
arr2 := [10]int{1, 9: 10, 4: 5}
fmt.Printf("%#v\n", arr2) 
// [10]int{1, 0, 0, 0, 5, 0, 0, 0, 0, 10}

/* Read from an Array */ 
arr := [...]string{ 
  "World", 
  "Hello", 
}
fmt.Println(arr[1], arr[0]) // Hello World
// Write
arr[0] = "Guys"
fmt.Println(arr[1], arr[0]) // Hello Guys
// Go runtime tracks the length internally, hence it's ok to use `len` as the Loop condition
fmt.Println(len(arr)) // 2
```

##### Slice

> Dynamic length ordered list 
```go
var arr []string
arr = append(arr, "Hello")
arr = append(arr, "World")
fmt.Println(arr) // [Hello World]
// one liner
arr = append(arr, "Hello", "World")


// Example:
package main
import (
  "fmt"
  "os"
)

func main() {
  args := getArgs()
  fmt.Println(args)
}

func getArgs() []string {
  // fmt.Println(os.Args)
  // Since the first arguments of the slice(os.Args) is the filename
  if len(os.Args) <= 1 {
    fmt.Printf("At least %v arguments are needed\n", 1)
    os.Exit(1)
  }

  var args []string
  for i := 1; i < len(os.Args); i++ {
    args = append(args, os.Args[i])
  }

  return args
}
// Run the below command
go run . Hello from the console
// [Hello from the console]


/* Combine two slices */
var slice1 []string
slice1 = append(slice1, "En", "Bn")
fmt.Println(slice1)

slice2 := []string{"Jp", "Fr"}
slice1 = append(slice1, slice2...)
fmt.Println(slice1) // [En Bn Jp Fr]

// sub slice
s := []int{1, 2, 3, 4, 5, 6, 7, 8, 9}
fmt.Println(s[2:]) // [3 4 5 6 7 8 9]
fmt.Println(s[:3]) // [1 2 3]
fmt.Println(s[2:6]) // [3 4 5 6]
fmt.Println(s[:]) // [1 2 3 4 5 6 7 8 9]


/* `make` function */
var s1 []int 
s2 := make([]int, 10)
s3 := make([]int, 10, 50)
fmt.Printf("s1: len = %v cap = %v\n", len(s1), cap(s1)) 
// s1: len = 0 cap = 0
fmt.Printf("s2: len = %v cap = %v\n", len(s2), cap(s2)) 
// s2: len = 10 cap = 10
fmt.Printf("s3: len = %v cap = %v\n", len(s3), cap(s3))
// s3: len = 10 cap = 50
```


##### Map

> key-value hash
```go
// map[<key_type>]<value_type>
users := map[string]string{
  "305": "Jorge",
  "204": "Bob",
  "631": "Jack",
}

users["405"] = "Don"
fmt.Println(users)
// map[204:Bob 305:Jorge 405:Don 631:Jack]

/* Reading from the map */
package main
import (
  "fmt"
)

func main() {
  users := getUsers()
  
  // `exists` is a boolean value that is true if a key exists in the map; otherwise, it's false
  user, exists := users["305"]
  fmt.Println(user, exists)
  // Jorge true

  user, exists = users["101"]
  fmt.Println(user, exists)
  //  false
}

func getUsers() map[string]string {
  return map[string]string {
    "305": "Jorge",
    "204": "Bob",
    "631": "Jack",
  }
}


/* Delete from the map */
// To delete we need to use the build-in `delete` function: delete(<map>, <key>)
delete(users, "305")
```


##### Custom Type

type <name> <type>

```go
type id string

// now we can use this `id` typed variable
var id1 id // default initial value will be ""
var id2 id = "1234-5678" // declared with an initial value
var id3 id // setting the value in the next line
id3 = "1234-5678"
```


##### Struct

Collections(Array, Slice, Map) are perfect for grouping values of the same type. For using different types of data in the same structure, we have a  new type `struct`.

```go
type user struct {
  name string 
  age int 
  balance float64
} 
```

Now we can use this many ways:
```go
u1 := user{ 
  name: "Jorge", 
  age: 31, 
  balance: 98.43,
} 

// un-assigned properties will use their zero value
u2 := user{
  age: 29,
  name: "Bob",
}

// we can also use the values only, we have maintian the order then
u3 := user{ 
  "Jack", 
  34, 
  0, 
} 

var u4 user
u4.name = "Don" 
u4.age = 35
u4.balance = 17.09

fmt.Println(u1, u2, u3, u4)
// {Jorge 31 98.43} {Bob 29 0} {Jack 34 0} {Don 35 17.09}
u2.name // Bob

/* Anonymous struct */
point := struct { 
    x int 
    y int 
} { 
    10, 
    10, 
  } 
// initialize with zero value and then assigning the value
point2 := struct { 
    x int 
    y int 
}{} 
point2.x = 10 
point2.y = 5


/* Struct embedding */
type name string
type size struct {
  width int
  height int
}
type embeded struct {
  name
  size
}

obj := embeded {}
obj.name = "Test"
obj.width = 35
obj.height = 35

obj2 := embeded { 
  name: "B",  
  size: size{ 
    width: 5, 
    height: 7, 
  }, 
} 

obj3 := embeded {} 
obj3.name = "C"
obj3.size.width = 20 
obj3.size.height = 30

fmt.Printf("obj: %v, name: %v, width: %v\n", obj, obj.name, obj.width) 
// obj: {Test {35 35}}, name: Test, width: 35
fmt.Printf("obj2: %v, name: %v, width: %v\n", obj2, obj2.name, obj2.width)
// obj2: {B {5 7}}, name: B, width: 5
fmt.Printf("obj3: %v, name: %v, width: %v\n", obj3, obj3.name, obj3.width)
// obj3: {C {20 30}}, name: C, width: 20
```
