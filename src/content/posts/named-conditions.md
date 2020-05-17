---
title: "Named Conditions"
summary: "Readable conditional statements in Go"
date: 2020-05-16T22:49:23-04:00
tags: ["go","programming"]
draft: false
---

Variable assignments in conditional statements are one of my favorite syntactic features of Go.
They help make complicated conditonal logic readable and self-documenting. 

Consider this code:

```go
if i == len(lines)-1 && line[len(line)-1] != '\n' {
    // ...
}
```

What's it doing? After a few seconds, you might recognize some common patterns and 
summarize the condition by saying "if the line is last and does not end with a newline..."


Here's a more difficult example [from the standard library](https://golang.org/src/image/jpeg/huffman.go):

```go
if h.lut[(d.bits.a>>uint32(d.bits.n-lutSize))&0xff] != 0 {
    // ...
}
```

Um...

<img src="/img/confused.jpg" alt="wut" class="img-small-center">

In languages that don't support assignments in conditional statements like Go, you're
probably stuck either wrapping the logic in its own function, or commenting the block
with a bit of explanation.

In Go, assigments in conditionals can be used to make the logic self-documenting, in
a pattern I like to call _named conditions_. Returning to our first example, we can name
the two conditions to allow the code to be understood immediately:

```go
if last, missingNewline := i == len(lines)-1, line[len(line)-1] != '\n'; last && missingNewline {
    // ...
}
```

Even if I have no need for those variables in the following block, named conditions
help the code read more naturally and let the next engineer grok my logic more quickly and accurately. 