# Object Properties

This section details the standards, rules, and guidelines regarding object properties.

[Public vs protected properties](#public-vs-protected-properties)
[Naming conventions](#naming-conventions)
[Setting property values](#setting-property-values)
[Getting property values](#getting-property-values)
[Lazy properties](#lazy-properties)
[Class properties](#class-properties)

# Public vs protected properties

Objects may have any number of public and protected (or private) properties. For the purposes of our coding standards,
we don't distinguish between protected and private properties, hence, they are considered the same thing.

The difference between public and protected properties is that public properties are accessible from outside the class
while protected properties are only accessible within the class and within child classes throughout the class hierarchy.
The only other difference is how they are named. Protected properties start with an underscore character `_` which make
it easy to tell if a property is public or protected.

# Naming conventions

Object properties should always start with a lower case letter (or an underscore and lower case letter if the property
is protected), should follow the camel case convention, and should be named after the data that it stores (a noun).

For example, you may have an object that has a `firstName` and a  `lastName` property. Notice that the two properties
are named after what they store.

Boolean typed properties must start with `is` followed by the name of the boolean value that they track. For example,
your object may have a property named `isEnabled`

Lazy properties (outlined below) must end with the word `Lazy`. For example, your object may have a lazy loaded property
named `_fullNameLazy` which is made up of the `firstName` and `lastName` properties and is only calculated when needed.

# Setting property values

All objects should appear to be immutable from outside the class which means that property values should not be changed
(at least not from outside the class). Instead, objects should be responsible for their own mutability if they are
mutable at all. We encourage object immutability, but sometimes objects need to be mutable and that's ok... just make
sure that the mutability of an object is internal. That being said, there are rules when it comes to setting new values
for object properties from outside the class.

Property values may only be set within the object constructor, [init methods](MethodTypes.md#init-methods),
[do methods](MethodTypes.md#do-methods), or [on methods](MethodTypes.md#on-methods). You may not set the values of a
property from outside the class or via any other method. Take the following code snippet as an example:

```javascript
// In this example, assume we have a class called Person that
// has a name property
class Person {

    // Init with a name
    func initWithName(name) {
        self.name = name
        return self
    }
    
    property name = ""
}

// Create our object instance
person = Person().initWithName("Jack")

// WRONG
// Mutating an object from outside the class is not allowed
person.name = "Jill"

// CORRECT
// Instead, call a *do method* to update the value if it exists
// This makes the object responsible for it's own mutation.
// Of course, you would need to add the "doUpdateName" method
// to the class definition above.
person.doUpdateName("Jill")
```

One more thing to consider is that all methods that update or set property values MUST make sure the value is valid
before setting the property value on the object. If the value is invalid, you should throw an exception. However, you
may also manipulate the value so that it passes validation before setting as well, but this behavior is not usually
recommended.

# Getting property values

Getting property values is simple. *Get the value... do with it what you will*.

The only rule here is that you may not get protected property values from outside the class. Protected properties should
only be accessible within the class (and child classes)

```javascript
// Our Person class definition...
// this time with a protected property called _password
class Person {

    // Init with a name
    func initWithName(name) {
        self.name = name
        
        return self
    }
    
    property name = ""
    
    property _password = "p@55w0rD"
    
    func isMatchesPassword(password) {
    
        // CORRECT
        // We can read the _password property within the class definition
        if password == self._password {
            return true
        } else {
            return false
        }
    }
}

// Create our object instance
person = Person().initWithName("Jack")

// WRONG
// Can't read _password from outside the class
password = person._password

// As per the class definition, we can check if the password matches though
isMatches = person.isMatchesPassword("Hello")

// isMatches is now false because it didn't match
```

# Lazy properties

Lazy properties are special in that they don't have a value until they are *requested* via getting the property (or
until the value is loaded internally due to some other circumstance). Different languages have different syntax as far
as how lazy properties are handled, but here is the basic pseudo code that explains how they work.

```javascript
// Assume our property is fullName

// We have a protected property that backs the normal property
// that ends with *Lazy*. It must be initialized to a null value
property _fullNameLazy = null

// Then we have the normal property which is a computed property behind the scenes
property fullName {

    // If the lazy property is null, we need to load it
    if self._fullNameLazy is null {
    
        // Load it with a give method.
        // We could also just load it right here, inline
        self._fullNameLazy = self._loadedFullName()
    }
    
    // Return the value
    return self._fullNameLazy
}

// Special give method that gives the loaded value
// this method must start with _loaded and end with the name of the property
// It may not take any parameters and may not throw an exception.
func _loadedFullName() {
    
    // Return the loaded value
    return self.firstName + " " + self.lastName
}
```

From the outside, it looks like a normal property and we can't tell the difference between a normal property and a lazy
property. Consider the following code snippet (written in pseudo code)

```javascript
// Our same ole Person class... with some changes
class Person {

    func initWithFirstName(firstName, lastName) {
        self.firstName = firstName
        self.lastName = lastName
        return self
    }

    // First name
    property firstName = ""
    
    // Last name
    property lastName = ""
    
    // Full name is lazy loaded
    property _fullNameLazy = null
    property fullName {
        if self._fullNameLazy is null {
        
            self._fullNameLazy = self._loadedFullName()
            
            // We could have also loaded the value inline like this
            // self._fullNameLazy = self.firstName + " " + self.lastName
        }
        return self._fullNameLazy
    }
    
    // Loader method
    func _loadedFullName() {
    
        // Return the loaded value
        return self.firstName + " " + self.lastName
    }
}

// From the outside, firstName, lastName, and fullName 
// simply look like normal properties

person = Person().initWithFirstName("Jack", "Smith")

// Prints "Jack"
print(person.firstName)

// Prints "Smith"
print(person.lastName)

// Prints "Jack Smith"
print(person.fullName)
```

**About the loader method**  
The loader method is a special kind of [give method](MethodTypes.md#give-methods) that *gives* the loaded value for a
lazy property. It follows the same rules as other *give methods* except that it takes no parameters and must be named
as `_loadedPropertyName`

**IMPORTANT** Some languages (namely Swift) support lazy properties natively and do not need a lot of the code mentioned
above. In languages like Swift, you would simply use the native language features for lazy properties like so:

```swift
// The person class, in Swift
class Person {

    // Init with a first and last name
    init(firstName: String, lastName: String) {
        self.firstName = firstName
        self.lastName = lastName
    }

    // The first name
    let firstName: String

    // The last name
    let lastName: String

    // The full name
    lazy var fullName: String = self._loadedFullName()
    func _loadedFullName() -> String { return self.firstName + " " + self.lastName }
}

// Create our person object
let person = Person(firstName: "Jack", lastName: "Smith")

// Prints "Jack"
print(person.firstName)

// Prints "Smith"
print(person.lastName)

// Prints "Jack Smith"
print(person.fullName)
```
