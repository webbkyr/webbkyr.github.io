---
layout: post
title:  Quick Tip - Debugging Go in VSCode
date:  2023-05-06
permalink: debugging-in-go
tags: debugging golang vscode

---

I've recently started writing services in Golang. Using the VSCode debugger has helped me learned (saved me a lot of time!). Lets quickly run through how to set it up.

Under the `Run And Debug` tab in VSCode, click `create a launch.json file`.

In this file is where we'll add configuration for our Go program. This is an example configuration for a simple program called `greetings`. The folder structure looks like this:

```
/greetings
  go.mod
  go.sum
  greetings_test.go
  greetings.go
  .env.development
  
```
And the `launch.json`:

{% highlight json %}
{
  "version": "0.2.0"
  "configurations": [
    "type": "go"
    "request": "launch",
    "name": "Greetings program" // this can be whatever you want
    "program": "${workspaceFolder}/",
    "envFile": "${workspaceFolder}/.env.development" // this is optional
  ]
}
{% endhighlight %}

And that's it! To test it out, set a breakpoint and run your tests(s)

greetings.go
{% highlight go %}
package greetings

import "fmt"

func Hello(name string) string {
	message := fmt.Sprintf("Hi, %v. Welcome!", name)
	return message
}

#=> prints 'Hi, NAME. Welcome!' to STDOUT.
{% endhighlight %}

greetings_test.go
{% highlight go %}
package greetings

import (
	"testing"
	"github.com/stretchr/testify/assert"
)

func TestGreeting(t *testing.T) {
	message := Hello("Kayla")
	expected_message := "Hi, Kayla. Welcome!"
	
  assert.Equal(t, expected_message, message)
}
{% endhighlight %}
