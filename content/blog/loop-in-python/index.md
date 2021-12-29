---
title: For Loop
date: "2021-12-28T13:50:32.169Z"
description: For Loop in Python vs Golang
---

The C style for loop is similar in Golang but different in Python. 

In Go, one can initiate an index and increment it until a certain condition is met. 

In Python, to generate an index for an iterable, one can use enumerate or range(len(iterable_item)) function.

To demonstrate:

```go
// golang
for i := 0; i < 5; i++ {
    fmt.print(i)
}
```

```python
# python
for item in range(5):
    print(item)
```

and in case item is an iterable, such as a string or a list.

```python
# python
s = "python"
for i, v in enumerate(s):
    print(i, v)    
```

The range functionality is not unique to Python. Golang, has the same capability as well, with one upside. The range returns both index and item.

```go
// golang
for i, v in range "Golang" {
    fmt.Printf("The index of character %c is %d\n", v, i)
    }
}
```

Finally, note that if a declared value is not used, go compiler throws an error, thus has to be omitted. Use underscore character to omit a value. 

Below, we are ignoring the index.

```go
// golang
for _, v in range "Text" {
    fmt.Print(v)
}

```
