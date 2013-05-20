Web applications in Go
======================

This intro will be mostly examples which show how to do it.

 ```go
package main

import (
  "fmt"
  "net/http"
)

func hello(w http.ResponseWriter, req *http.Request) {
  fmt.Fprintln(w, "Hello World!")
}

func main() {
  http.HandleFunc("/", hello)
  if err := http.ListenAndServe(":8080", nil); err != nil {
      panic(err)
  }
}
 ```
