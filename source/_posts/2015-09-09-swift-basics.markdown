---
layout: post
title: Swift Basics
date: 2015-09-09 18:48:12 -0500
comments: true
categories: swift
---

# String Interpolation

``` swift

var name = "Eric"

println("Hello, \(name)")

// "Hello, Eric"
```

# Dictionaries

``` swift
var response = ["id":14,"email":"test@email.com"]

let id = response["id"]

// 14

let email = response["email"]

// "test@email.com"

```

# Tuples

### Unnamed Tuples

``` swift

var coordinates = (100, 999)

let (lat, lon) = coordinates

println("Latitude is \(lat)")

// "Latitude is 100"

println("Longtitude is \(lon)")

// Longitude is 999
```

### Named Tuples

``` swift
var response = (code: 200, message: "All good")

response.0
// 200

response.1
// "All good"

response.code
// 200

respsonse.message
// "All good"
```

# Swift Classes

### Creating a class

``` swift Person.swift
   
class Person: NSObject {
  var name: String
  var email: String
  var zip: Int
  
  init(name: String, email: String, zip: Int) {
    self.name = name
    self.email = email
    self.zip = zip
    super.init()
  }
}
```

### Create data for Person Class

``` swift SampleData.swift

let personData = [
  Person(name: "Eric", email: "eric.iacutone@gmail.com", zip: 12345)
]
```

### Instantiating Person Class

``` swift PersonViewController.swift

var person:Array = personData

```

Now, if you copy and paste the the files into a XCode Playground, you can call methods on your person object.

``` swift Playground.swift

person.count
// 1

person.first?.zip
// 12345
```

# Enums

An enumeration defines a common type for a group of related values and enables you to work with those values in a type-safe way within your code. - [Swift docs](https://developer.apple.com/library/prerelease/ios/documentation/Swift/Conceptual/Swift_Programming_Language/Enumerations.html)

Use an enum when you need a consistent data value.

``` swift

enum CountryType:String {
    case UnitedStates = "United States"
    case Spain = "Spain"
    
    init() {
      self = .UnitedStates
    }
}

var type = CountryType.UnitedStates.rawValue

// "United States"
```

# Structs

A struct allows you to create a structured data type which provides storage of data using properties and extending its functionality via methods. -[Tree House](http://blog.teamtreehouse.com/enums-structs-swift)

``` swift

struct Person {
    var name: String
    var email: String
    var country: CountryType
}

var person = Person(name: "Eric", email: "eric.iacutone@gmail.com", country: CountryType.UnitedStates)

person.name

// "Eric"

person.country.rawValue

// "United States"
```

