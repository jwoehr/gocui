*This fine piece of open source software is forked into github.com/jwoehr*
*solely so that we can select further changes which might affect*
*github.com/jwoehr/goublu. Copious thanks to the original author(s).*
*Jack Woehr 2017-06-21*

# GOCUI - Go Console User Interface

Minimalist Go package aimed at creating Console User Interfaces.

## Features

* Minimalist API.
* Views (the "windows" in the GUI) implement the interface io.ReadWriter.
* Support for overlapping views.
* The GUI can be modified at runtime (concurrent-safe).
* Global and view-level keybindings.
* Mouse support.
* Colored text.
* Customizable edition mode.
* Easy to build reusable widgets, complex layouts...

## Installation

Execute:

```
$ go get github.com/jwoehr/gocui
```

## Documentation

Execute:

```
$ go doc github.com/jwoehr/gocui
```

## Example

```go
package main

import (
	"fmt"
	"log"

	"github.com/jwoehr/gocui"
)

func main() {
	g, err := gocui.NewGui(gocui.OutputNormal)
	if err != nil {
		log.Panicln(err)
	}
	defer g.Close()

	g.SetManagerFunc(layout)

	if err := g.SetKeybinding("", gocui.KeyCtrlC, gocui.ModNone, quit); err != nil {
		log.Panicln(err)
	}

	if err := g.MainLoop(); err != nil && err != gocui.ErrQuit {
		log.Panicln(err)
	}
}

func layout(g *gocui.Gui) error {
	maxX, maxY := g.Size()
	if v, err := g.SetView("hello", maxX/2-7, maxY/2, maxX/2+7, maxY/2+2); err != nil {
		if err != gocui.ErrUnknownView {
			return err
		}
		fmt.Fprintln(v, "Hello world!")
	}
	return nil
}

func quit(g *gocui.Gui, v *gocui.View) error {
	return gocui.ErrQuit
}
```