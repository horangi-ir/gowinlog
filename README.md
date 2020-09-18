# gowinlog
Go library for subscribing to the Windows Event Log.

Godocs
=======

godoc is not proper, look at the example

Installation
=======

just go get the thing

Features
========

- Includes wrapper for wevtapi.dll, and a high level API
- Supports bookmarks for resuming consumption
- Filter events using XPath expressions 

Usage
=======

``` Go
package main

import (
  "fmt"
  "github.com/alanctgardner/gowinlog"
)

func main() {
  watcher, err := winlog.NewWinLogWatcher()
  if err != nil {
    fmt.Printf("Couldn't create watcher: %v\n", err)
    return
  }
  // Recieve any future messages on the Application channel
  // "*" doesn't filter by any fields of the event
  watcher.SubscribeFromNow("Application", "*")
  for {
    select {
    case evt := <- watcher.Event():
      // Print the event struct
      fmt.Printf("Event: %v\n", evt)
    case err := <- watcher.Error():
      fmt.Printf("Error: %v\n\n", err)
    }
  }
}
```

Low-level API
------

`winevt.go` provides wrappers around the relevant functions in `wevtapi.dll`.
