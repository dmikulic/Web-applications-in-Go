Web applications in Go
======================

This intro consists mostly of examples which show how to do it.

#### 1. Hello world

main.go
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
 
#### 2. Adding basic templates

```go
package main

import (
        "html/template"
        "net/http"
)

var index = template.Must(template.ParseFiles(
        "templates/_base.html",
        "templates/index.html",
))

func hello(w http.ResponseWriter, req *http.Request) {
        if err := index.Execute(w, nil); err != nil {
                http.Error(w, err.Error(), http.StatusInternalServerError)
        }
}

func main() {
        http.HandleFunc("/", hello)
        if err := http.ListenAndServe(":8080", nil); err != nil {
                panic(err)
        }
}
```
