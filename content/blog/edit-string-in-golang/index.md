---
title: Edit Strings in Go
date: "2021-12-28T15:50:32.169Z"
description: "Strings are immutable in golang. How can you edit a character in string?"
---

In Golang, strings are immutable.

Unlike Python or Javascript, strings cannot be edited by accessing its index.

To better explain in Python, the following is possible, but not in Golang.

```python
# python

s = 'string'
s[0] = 'S'
print(s) # prints String
```

So in Golang, rune or slicer method can be used.

**Rune option**

```go
// golang
func replace(s string, index int, c rune) string {
    s_rune := []rune(s)
    s_rune[index] = c
    return string(s_rune)
}
```

such that you can use it as 

```go
// golang 
package main

import "fmt"

func main() {
    r = replace("string", 0, 'S')
    fmt.Println(r) // String
}

...
// func replace here
```

Alternatively,
**Slice option**:

```go
// golang
myString := "string"
myString = "S" + myString[1:]
```