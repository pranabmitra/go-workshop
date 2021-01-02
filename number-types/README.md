#### Types

##### Integer types
| Type     | Description                                         |
| -------- | --------------------------------------------------- |
| uint8    | Unsigned 8-bit intergers (0 to 255)                 |
| uint16   | Unsigned 16-bit intergers (0 to 65535)              |
| uint32   | Unsigned 32-bit intergers (0 to 4294967295)         |
| uint64   | Unsigned 64-bit intergers (0 to 4294967295)         |
| int8     | Signed 8-bit intergers (-128 to 127)                |
| int16    | Signed 16-bit intergers (-32768 to 32767)           |
| int32    | Signed 32-bit intergers (-2147483648 to 2147483647) |
| int64    | Signed 64-bit intergers (-128 to 127)               |
| byte     | Alias for uint8                                     |
| rune     | Alias for int32                                     |

##### Special Types
| Type     | Description          |
| -------- | -------------------- |
| uint     | Either 32 or 64 bits |
| int      | Either 32 or 64 bits |

> uint and int are either 32 or 64 bits depending on whether we compile our code for a 32-bit system or a 64-bit system. It's rare nowadays to run applications on 32-bit system, systems so most of the time they are 64 bits.

> An int on a 64-bit system is not an int64. While these two types are identical, they are not the same integer type, and we can't use them together.

We should use typed variable properly to avoid memory issues in the application. We can have a loot at the following example to know the heap memory used by the program:

```go
package main 
import ( 
  "fmt" 
  "runtime" 
)

func main() { 
  var list []int 
  // var list []int8
  for i := 0; i < 10000000; i++ { 
    list = append(list, 100) 
  }
   
  var m runtime.MemStats 
  runtime.ReadMemStats(&m) 
  fmt.Printf("TotalAlloc (Heap) = %v MiB\n", m.TotalAlloc/1024/1024) 
} 

// we will get the following for those types
/*
 * TotalAlloc (Heap) = 403 MiB // var list []int 
 * TotalAlloc (Heap) = 54 MiB // var list []int8
 */
```


##### Floating Point

Go has two floating-point number types, `float32` and `float64`.
Typically `float64` could be the first choice unless we need to be more memory efficient.

```go
var a int = 100
var b float32 = 100
var c float64 = 100

fmt.Println(a / 3) // 33
fmt.Println(b / 3) // 33.333332
fmt.Println(c / 3) // 33.333333333333336
```


##### Big Numbers

If we need to use a number higher and lower than `int64` or `uint64`, we can use `math/big` package.
```go
package main
import (
  "fmt"
  "math"
  "math/big"
)

func main() {
  // Initialize with Go's highest possible number value
  bigA := big.NewInt(math.MaxInt64)
  // Add 1 to our big int
  bigA.Add(bigA, big.NewInt(1))
  fmt.Println("Big Int : ", bigA.String())
}
```

##### Text

There are two kinds of string literals in Go.
* Raw – defined by wrapping text in a pair of `
* Interpreted – defined by surrounding the text in a pair of "

```go
comment1 := `In "Windows" the user directory is "C:\Users\"` 
comment2 := "In \"Windows\" the user directory is \"C:\\Users\\\"" 
```

For printing multi-byte characters, we have to use `rune`. E.g.
```go
username := "Über"  // Ü - is encoded with more than one byte
runes := []rune(username)
for i := 0; i < len(runes); i++ { 
  fmt.Print(string(runes[i]), " ") 
}

// we can use `range` to print each multi-byte char
for index, runeVal := range username {
  fmt.Println(index, string(runeVal))
}
```

> `nil` is not a type but a special value in Go. It represents an empty value of no type.