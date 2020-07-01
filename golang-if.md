# If statement in golang

The `if` can assign new variable in the statement in Go

Like this:

```go
if [initializer;] condition {
  //code here
}
```

Initializer can use to execute one line simple code. The only posible usecase I can think of is create variable, for example:

```go
// before
result := list[0]
if result > 0 {
  return result;
}

// after
if result := list[0]; result > 0 {
  return result;
}
```

1. can only use `:=` to assign value here, because `if` initializer not support to use `var`.

2. can only access variable inside the block of `if` and it's `else`