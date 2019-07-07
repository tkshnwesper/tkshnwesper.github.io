---
layout: post
title:  "The Mysterious Go Slices"
categories: [programming]
tags: [golang, slices]
---

## Introduction

I'm (re-)learning Golang presently from the same tutorial that I had followed 3 years ago which taught me to code [Niec](https://github.com/tkshnwesper/niec).

That tutorial was none other than...

Wait for it...

[A tour of Go][A tour of Go]

It's just too good for words! Would definitely recommend.

Well, anyway coming to the point. I wanted to take a few moments to highlight the mysterious behaviors and edge-cases of Golang slices. A lot of the examples from here are taken from [A tour of Go][A tour of Go].

I am going to assume that you know about Go slices already, if not you could read up about the basics from over [here](https://tour.golang.org/moretypes/7).

## Pre-requisites

Before we begin, let's throw this helper function into our arsenal and assume that it is present as we run through all our examples.

```go
func printSlice(s []int) {
  fmt.Printf("len=%d cap=%d %v\n", len(s), cap(s), s)
}
```

It would print the output of the following program:

```go
func main() {
  s := []int{2, 3, 5, 7, 11, 13}
  printSlice(s)
}
```

as

```text
len=6 cap=6 [2 3 5 7 11 13]
```

## References

You probably might have encountered something on the lines of:

> Slices refer back to the array from which they were sliced from

Now I really want to bring this concept to light, because these references do not seem like any of the references that I have encountered in the past.

Take a look at this.

```go
func main() {
  s := []int{2, 3, 5, 7, 11, 13}
  printSlice(s)
}
```

That would print

```text
len=6 cap=6 [2 3 5 7 11 13]
```

It basically creates an **array** in the background and returns a slice of that array. Coincidentally (or not) the slice and array have the same **capacity** and **length**.

Cool, let's slice it and give it a length of 0

```go
func main() {
  s := []int{2, 3, 5, 7, 11, 13}

  s = s[:0]
  printSlice(s)
}
```

Output:

```text
len=0 cap=6 []
```

(Note that the capacity still remains intact.)

**Gasp! Whatever happened to all the elements in the array?**

Well, don't be alarmed, because all the elements are still hanging around 😉

```go
func main() {
  s := []int{2, 3, 5, 7, 11, 13}
  
  s = s[:0]

  s = s[:6]
  printSlice(s)
}
```

Output:

```text
len=6 cap=6 [2 3 5 7 11 13]
```

Based on this observation, I was able to conclude that

> a slice is a window to an array.

## Appending

Alright, let's look at the fun that happens when we append to a slice.

```go
func main() {
  var s []int
  
  s = append(s, 0, 1, 2, 3, 4, 5)
  printSlice(s)
}
```

Gives an output of

```text
len=6 cap=8 [0 1 2 3 4 5]
```

So based on what we have learned about slices, we know that they are references (or windows) to underlying arrays.

This might come as a pretty obvious fact to you:

```go
func main() {
  var s []int
  
  s = append(s, 0, 1, 2, 3, 4, 5)
  t := append(s, 6)

  printSlice(s)
  printSlice(t)
}
```

Output:

```text
len=6 cap=8 [0 1 2 3 4 5]
len=7 cap=8 [0 1 2 3 4 5 6]
```

The point of the above example was to check whether appending an element to the slice and assigning the slice to a different variable (`t` in this case) would get the same result.

However, that did not seem to be the case 🤔

But that does not mean that the reference does not hold,

```go
func main() {
  var s []int
  
  s = append(s, 0, 1, 2, 3, 4, 5)
  t := append(s, 6)

  printSlice(s[:7])
  printSlice(t)
}
```

Output:

```text
len=7 cap=8 [0 1 2 3 4 5 6]
len=7 cap=8 [0 1 2 3 4 5 6]
```

The above example shows that the referential integrity still holds. `s` knows about the newly appended element!

If you thought all that seemed normal, take a look at this:

```go
func main() {
  var s []int
  
  s = append(s, 0, 1, 2, 3, 4, 5)
  t := append(s, 6, 7, 8)

  printSlice(s)
  printSlice(t)
}
```

Here's the output:

```text
len=6 cap=8 [0 1 2 3 4 5]
len=9 cap=16 [0 1 2 3 4 5 6 7 8]
```

Hmm... 😕

If that caught you unawares, I would like to quote a line from [here](https://tour.golang.org/moretypes/15).

> If the backing array of s is too small to fit all the given values a bigger array will be allocated. The returned slice will point to the newly allocated array.

Well, things are starting to make little sense now. We assigned a slice that exceeded the capacity of `s` to `t`. So `t` would potentially contain a reference to a newly created array and `s` to the original.

Lastly, let us try and expand `s` to its maximum capacity to check whether any of the appended elements are present.

```go
func main() {
  var s []int
  
  s = append(s, 0, 1, 2, 3, 4, 5)
  t := append(s, 6, 7, 8)

  printSlice(s[:8])
  printSlice(t)
}
```

Output:

```text
len=8 cap=8 [0 1 2 3 4 5 0 0]
len=9 cap=16 [0 1 2 3 4 5 6 7 8]
```

😖 Okay... I need to take a break now (just kidding)!

That may not have appeared to be very obvious right off the bat, but think about it... Even though we are appending elements to `s`, Go realizes that there's no way it would be able to fit all those elements inside the array referred to by `s` since the capacity of `s` is only `8`, while we need to fit in `9` elements. As a result, it simple allocates another array into which it copies the original values as well as the new values. That is the reason `s` remains untouched and a slice of the new array is stored in `t`.

Hurray! 🎊

[A tour of Go]: https://tour.golang.org
