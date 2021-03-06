# Fractal

[![Travis](https://img.shields.io/travis/ddliu/fractal.svg?style=flat-square)](https://travis-ci.org/ddliu/fractal)
[![godoc](https://img.shields.io/badge/godoc-reference-blue.svg?style=flat-square)](https://godoc.org/github.com/ddliu/fractal)
[![License](https://img.shields.io/badge/license-MIT-blue.svg?style=flat-square)](LICENSE)
[![Go Report Card](https://goreportcard.com/badge/github.com/ddliu/fractal)](https://goreportcard.com/report/github.com/ddliu/fractal)
[![cover.run](https://cover.run/go/github.com/ddliu/fractal.svg?style=flat&tag=golang-1.10)](https://cover.run/go?tag=golang-1.10&repo=github.com%2Fddliu%2Ffractal)


Fractal is a Go package that makes it easy to work with dynamic and nested data types, with encoding/decoding support.

## Features

- Nested data type
- Dot path support
- JSON encoding/decoding
- Simple template replacement
- Inplace update
- Common data type support: Struct, Map...

## Install

```
go get -u github.com/ddliu/fractal
```

## Usage

Work with struct

```go
data := myStruct {
    Key1: "Value1",
    Key2: anotherStruct {
        Key3: "Value3"
    }
}

// Create context
ctx := fractal.New(data)
println(ctx.String("Key2.Key3"))
// output: Value3
```

Work with json

```go
ctx := fractal.FromJson([]byte(`{"key1": "value1", "key2": {"key3": "value3"}}`))
println(ctx.String("key2.key3"))
```

Or with JSON unmarshal

```go
var ctx fractal.Context
json.Unmarshal([]byte(`{"key1": "value1", "key2": {"key3": "value3"}}`), &ctx)
println(ctx.String("key2.key3"))
```

Work with map

```go
ctx := fractal.New(map[string]interface{
    "key1": "value1",
    "key2": "value2",
})
```

Update:

```go
ctx.SetValue("key2.new_key", 3)
```

Simple template:

```go
tpl := `Author: ${author.name}; License: ${license}; `

c := New(nil)
c.SetValue("author", map[string]string{
    "name":  "Dong",
    "email": "test@example.com",
})

c.SetValue("license", "MIT")

println(c.Tpl(tpl))

// output: Author: Dong; License: MIT; 
```

## Dot Notation

- `a.b`: Notation access
- `a.b.1`: Array index
- `a.b.length()`: Length of array or object