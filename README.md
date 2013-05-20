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

main.go
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

templates/_base.html
```html
<!DOCTYPE html>
<html lang="en">
    <head>
        <title>{{ template "title" . }}</title>
    </head>
    <body>
        <section id="contents">
            {{ template "content" . }}
        </section>
        <footer id="footer">
            My site - 2013
        </footer>
    </body>
</html>
```

templates/index.html
```html
{{ define "title" }}Index page{{ end }}

{{ define "content" }}
    <h1>Index page</h1>
    <hr/>
    <h3>Some content</h3>
{{ end }}
```
