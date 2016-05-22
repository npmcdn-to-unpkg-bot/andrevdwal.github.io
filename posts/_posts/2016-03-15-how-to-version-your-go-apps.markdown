---
layout: post
title:  "How to version your Go application"
date:   2016-03-15
tags:   go golang code version
---
So you want to version your application and have it output that version when you run it with a `version` argument like so:

```
$ .\main version  (or "main.exe version" for you Windows guys)
```

Well, it's actually pretty simple. What you do is declare a global string variable (we'll call it version) and give it a value when it compiles. Create a `main.go` file:

```go
package main

import "fmt"

var version string

func main() {
    fmt.Println(version)
}
```
If we run it `go run main.go` it prints out...nothing. Bummer, but expected.

Now run it with the ldflags set: `go run -ldflags "-X main.version=1.0.2" main.go` and it will print out 1.0.2. So if you were to include this flag when you build the binary the version variable will have the value you gave it FOREVER!!

But what about the version argument? Glad you asked. All we need is a very impressive if statement at the start of our main method that checks if the second argument's value is "version":

```go
package main

import (
	"fmt"
	"os"
)

var version string

func main() {
	if len(os.Args) > 1 && os.Args[1] == "version" {
		fmt.Println("version", version)
		return
	}

	fmt.Println("do other stuff")
}
```

Now build and run it:

```
$ go build -ldflags "-X main.version=1.0.2" main.go
$ .\main version
version 1.0.2
```

I know: genius! Hope it helps somebody REALLY impress their boss!
