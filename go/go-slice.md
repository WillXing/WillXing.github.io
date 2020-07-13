# Go array and slice

## Array

Initialize array in Go

```go
arr := [3]int{1,2,3}
```

Array can create struct type when init

```go
arr := [2]struct{
  X int
  Y int
}{
  {1,2}
  {3,4}
}
```

## Slice

Slice is a reference of an array, but also can initialize by it self.

```go
arr := [3]int{1,2,3}
slice := arr[0:2] // {1,2}
slice[0] = 10
// arr[0] == 10 as well.

slice1 := arr[:2] // {1,2}
slice2 := arr[0:] // {1,2,3}
```

also, create a slice by itself

```go
slice := []int{1,2,3}
```

So the difference between slice and array I think slice is dynamic size.

Which I think it might be the way to create stack/queue.

### Capacity & Length

Let's see following code:

```go
slice := []int{1,2,3}
// capacity is 3. length is 3
slice = append(slice, 4)
// capacity: 6, length 4
// now the array for the slice is {1,2,3,4}
// slice is {1,2,3,4}
slice = slice[:1]
// capacity: 6, length 1
// slice is {1}
slice = append(slice, 5)
// capacity: 6, length 2
// now the array for the slice is {1,5,3,4}
// slice is {1,5}
```

My understanding is:
1. Capacity is the underlying array size, it will change when we appened new item into it, when oversize, it doubled.
2. Append will change capacity of slice.
3. Length is the items in current slice.

When capacity and length are 0: `slice == nil`

### `Make` slice

Can use `make` function to build a slice, this can allow to set length and capacity without initialize any value
```go
slice := make([]int, 1, 2)
// length 1, capacity 2
```

## Iterate array or slice

Use `range`

```go
for index, value := range slice {
  fmt.Printf("index: %d, value: %d", index, value)
}

for _, value := range slice {
  // code
  // _ to skip read index
  // same way can skip value
}
```

## [Exercise](https://tour.golang.org/moretypes/18)

```go
func Pic(dx, dy int) [][]uint8 {
	slice := make([][]uint8, dy)
	
	for y := range slice {
		innerSlice := make([]uint8, dx)
		for x := range innerSlice {
			innerSlice[x] = uint8(x * y)
		}
		slice[y] = innerSlice
	}
	return slice
}

func main() {
	pic.Show(Pic)
}
```
