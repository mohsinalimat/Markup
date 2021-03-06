# Markup

<!--
Pending Swift 5.2 support
![CI][ci badge]
[![Documentation][documentation badge]][documentation]
-->

A Swift package for working with HTML, XML, and other markup languages,
based on [libxml2][libxml2].

**This project is under active development and is not ready for production use.**

## Features

- [x] HTML Support
- [x] Basic XML Support
- [x] XPath Expression Evaluation
- [ ] CSS Selector to XPath Functionality*
- [ ] XML Namespace Support*
- [ ] DTD and Relax-NG Validation*
- [ ] XInclude Support*
- [ ] XSLT Support*
- [ ] SAX Parser Interface*
- [ ] HTML and XML Function Builder Interfaces*

> \* Coming soon!

## Requirements

- Swift 5.2+ 👈❗️

## Usage

### XML

#### Parsing & Introspection

```swift
import XML

let xml = #"""
<?xml version="1.0" encoding="UTF-8"?>
<!-- begin greeting -->
<greeting>Hello!</greeting>
<!-- end greeting -->
"""#

let document = try XML.Document(string: xml)!
document.root?.name // "greeting"
document.root?.content // "Hello!"

document.children.count // 3 (two comment nodes and one element node)
document.root?.children.count // 1 (one text node)
```

#### Searching and XPath Expression Evaluation

```swift
document.search("//greeting").count // 1
document.evaluate("//greeting/text()") // .string("Hello!")
```

#### Modification

```swift
for case let comment as Comment in document.children {
    comment.remove()
}

document.root?.name = "valediction"
document.root?["lang"] = "it"
document.root?.content = "Arrivederci!"

document.description // =>
/*
<?xml version="1.0" encoding="UTF-8"?>
<valediction lang="it">Arrivederci!</valediction>

*/
```

* * *

### HTML

#### Parsing & Introspection

```swift
import HTML

let html = #"""
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Welcome</title>
</head>
<body>
    <p>Hello, world!</p>
</body>
</html>
"""#

let document = try HTML.Document(string: html)!
document.body?.children.count // 1 (one element node)
document.body?.children.first?.name // "p"
document.body?.children.first?.text // "Hello, world!"
```

#### Searching and XPath Expression Evaluation

```swift
document.search("/body/p").count // 1
document.search("/body/p").first?.xpath // "/body/p[0]"
document.evaluate("/body/p/text()") // .string("Hello, world!")
```

#### Creation and Modification

```swift
let div = Element(name: "div")
div["class"] = "wrapper"
if let p = document.search("/body/p").first {
    p.wrap(inside: div)
}

document.body?.description // =>
/*
<div class="wrapper">
    <p>Hello, world!</p>
</div>
*/
```

## Installation

### Swift Package Manager

First, add the Markup package to your target dependencies in `Package.swift`:

```swift
import PackageDescription

let package = Package(
  name: "YourProject",
  dependencies: [
    .package(
        url: "https://github.com/SwiftDocOrg/Markup",
        from: "0.0.1"
    ),
  ]
)
```

Next, add the `HTML` and/or `XML` modules 
as dependencies to your targets as needed:

```swift
targets: [
.target(
    name: "YourProject",
    dependencies: ["HTML", "XML"]),
```

Finally, run the `swift build` command to build your project.

## License

MIT

## Contact

Mattt ([@mattt](https://twitter.com/mattt))

[libxml2]: http://xmlsoft.org
[ci badge]: https://github.com/SwiftDocOrg/Markup/workflows/CI/badge.svg
[documentation badge]: https://github.com/SwiftDocOrg/Markup/workflows/Documentation/badge.svg
[documentation]: https://github.com/SwiftDocOrg/Markup/wiki
