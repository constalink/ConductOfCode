# Method Types

Class methods are powerful and can do many things like take many parameters, return a value, throw an exception, modify
object properties, validate data, load and save data, and so much more. The problem with this is that under normal
circumstances, it's impossible to easily tell what a method will do when called. For this reason, we have created a set
of rules and restrictions for your methods. This includes what they are allowed to do and how they should be named and
called.

In general, every single method that you add to your classes should be categorized as one of the method types outlined
here. Each type of method has different requirements, restrictions, etc.

* [Public methods vs protected methods](#public-methods-vs-protected-methods)
* [Constructor methods](#constructor-methods)
* [Init methods](#init-methods)
* [Give methods](#give-methods)
* [Validate methods](#validate-methods)
* [Do methods](#do-methods)
* [On methods](#on-methods)

# Public methods vs protected methods

Public methods should start with a lower case letter while protected (or private) methods should start with an
underscore and lower case letter. Here are some examples:

        // Public method
        func myPublicMethod() { }
        
        // Protected method
        func _myProtectedMethod() { }

# Constructor methods

Constructor methods are called when an object instance is created. Most languages have a single constructor that allows
you to set default values for your properties. Here are the rules for constructor methods.

* Constructors may not take any parameters, ever.

* The only thing you can do in a constructor is set default values for properties, nothing more.

* Object initialization and validation should never be performed in the constructor method. That job is left to init
  methods instead.

# Init methods

Init methods are used to initialize your objects. There are 2 types of init methods.  
**Convenience init methods** and **Designated init methods**.  
Object initialization is a complex process and for this reason, there are many rules that you must follow when creating
your init methods.

**Designated init methods** are the primary initializers for the class. They make sure that all properties are
initialized and have proper values before returning. They are also responsible for calling a designated initializer
from the immediate super class so that initialization can continue up the inheritance hierarchy. For the most part
a class should have a single designated init method. It is possible to have multiple designated init methods, but you
must make sure that no mater what designated init method is called, the object ends up in a valid state after return.
Here are a list of rules for designated init methods:

* Must make sure that all properties are initialized and have a proper value before returning.

* Must make sure that the object is in a valid state before returning.

* Must call a designated init method of the immediate super (parent) class before returning. This gives the super class
  a chance to initialize all it's properties as well and makes sure that initialization continues up the class
  hierarchy.
  
* Must take care of validation to make sure that all values are correct before returning.

* Must throw an exception if validation fails or the object cannot be put into a valid state for whatever reason.

* Every class must have at least 1 designated init method. In some cases, this requirement can be satisfied via method
  inheritance.

**Convenience init methods** are the secondary initializers for the class. They exist in order to *manipulate* their
input so that it can be safely passed to an appropriate designated init method. Convenience init methods are optional
and your class might not have any convenience initializers. If your class does define convenience init methods, it can
have many of them depending on how you want to initialize your class. Create convenience init methods as a shortcut,
a common use case, or perhaps to convert or manipulate the input into the correct format for a designated init method.

For example: A `Date` object may have a designated init method that takes a year, month, and day. You may also have a
convenience init method that takes a date string in the format of `YYYY-MM-DD`. The convenience init method would then
convert the string into a year, month, and day, then pass that data to the designated init method. Here are a list of
rules for convenience init methods.

* Must call another init method from the same class. This can either be another convenience init method or a designated
  init method.
  
* Must never call a super init method

* Must throw an exception if the input is not valid or if another init method from the same class cannot be called.

Designated init methods and convenience init methods have a delicate relationship and must follow certain rules in order
to ensure initialization consistency. Here are a list of rules that your init methods must follow

* A designated init method MUST call a designated init method from it's immediate super class. (unless it's a base
  class)

* A convenience init method MUST call another init method from the **SAME** class, not from a super class.

* A convenience init method MUST ultimately call a designated init method from the **SAME** class, not from a super
  class. (Only designated init methods may call a designated init method from a super class... in fact, they have to)

An easy way to remember these rules are:

* Designated init methods always call up to the super class.

* Convenience init methods always call across within the same class.

Here is a quick illustration that outlines these rules
![alt text](initializerDelegation01_2x.png "Init method illustration")
(This illustration was taken from the Swift Programming Language ePublication at
https://swift.org/documentation/TheSwiftProgrammingLanguage(Swift2.2).epub)

Wait.... we're not done. There are a few other things to consider or *rules* for your init methods.

* No object should be used at all until it is initialized.

* Init methods can take any number of parameters.

* Upon return, the object should be in a valid state and completely ready for use.

* If the object cannot be put into a valid state with the given input, an exception must be thrown.

* Abstract or base classes may not have any public init methods. All their init methods must be protected and may not be
  called from the outside.

* Init methods with no parameters must be named `init` if public or `_init` if protected. If they have at least 1
parameter, they must start with `initWith` or `_initWith` and should end with the name of the first parameter. Here are
some examples (Written in pseudo code)

         // Define your class
         class MyClass:
         
             // Public init method
             func init() {
                 // init code here
                 return self
             }
             
             // Protected init method
             func _init() {
                 // init code here
                 return self
             }
             
             // Public init method with a single parameter
             func initWithName(name) {
                 // init code here
                 return self
             }
             
             // Public init method with multiple parameters
             func initWithPrefix(prefix, name, suffix) {
                 // init code here
                 return self
             }
             
             // Protected init method with a single parameter
             func _initWithFullName(fullName) {
                 // init code here
                 return self
             }
             
             // Protected init method with multiple parameters
             func _initWithFirstName(firstName, lastName) {
                 // init code here
                 return self
             }
         }
  
* For the most part, all init methods should return the instance itself. This allows for easy object creation and usage.
  Consider this code (Written in pseudo code)
  
         // Create an object
         myInstance = MyObject()
         
         // Initialize the object
         myInstance.initWithName("My Name")
         
         // Now the instance is initialized and ready for use, but it takes 2 lines of code to create and init.
         // A better solution is to create and init all in the same line
         
         myInstance = MyObject().initWithName("My Name")
         
         // Now it's ready for use and easier to read
  
  The exception to this rule is the Swift programming language which allows many init methods and they don't return
  anything. Also, each programming language is a bit different, so you may want to take a look at some existing init
  code to see how it's done.

**Init method summary**

* **Naming convention** - Must be named `init` or `_init` if no parameters or `initWith` / `_initWith` if there is at
  least 1 parameter. (An exception is the Swift programming language which just used `init` for all init methods and the
  difference lies in the signature of the method)

* **Parameters** - May take 0 or more parameters

* **Return value** - Must return the object instance itself (An exception is the Swift programming language which
  doesn't return anything)
  
* **Throws** - May throw an exception if the object couldn't be put in a valid state

* **Call** - Must call another init method until a base designated init method is called. May call *validate* methods to
  validate values before returning. May not call any other methods.

* **State** - Designated init methods MUST initialize object state. Convenience init methods MAY initialize object state
  after a designated init method is called.

# Give methods

Give methods are simple. They simply take 1 or more parameters (in most cases) and return a value. Give methods are
descriptively named after what they return. For example, if the method takes an integer value and returns the square
root of that integer, it's signature might be something like: `func squareRootOfIntValue(intValue) { }`

Here are some rules regarding give methods

* Must take 1 or more parameters (An exception to this rule is where a programming language does not have computed
  properties. In that case, you can use give methods with no parameters as computed properties. [Learn more about
  properties here](Properties.md))

* Must be named descriptively after what it is they return along with the name of the first parameter of the method.
  They must never start with a verb like `do`, `init`, `set`, `load`, `save`. The official format is a noun, followed by
  a preposition, then the name of the first parameter. If the method does not take any parameters (in the case of using
  give methods in place of computed properties) then the method should just be named after what it returns. Rather than
  describing the naming rules in detail, we are just going to give you some signature examples using pseudo code:
  
        // WRONG
        // The name doesn't include the first parameter
        func squareRootOf(intValue) { }
        
        // WRONG
        // The name doesn't include a preposition like "Of", "With", "At", "For", etc.
        func squareRootIntValue(intValue)
        
        // CORRECT
        func squareRootOfIntValue(intValue) { }
        
        // WRONG
        // The name starts with a verb
        func saveName(key) { }
        
        // WRONG
        // The name doesn't include a preposition or first parameter
        func savedName(key) { }
        
        // WRONG
        // The name doesn't include a preposition
        func savedNameKey(key) { }
        
        // CORRECT
        func savedNameForKey(key) { }
        
        // Here's an example with multiple parameters
        // Notice that the 2nd and 3rd parameters are not part of the method name
        func fullNameForFirstName(firstName, lastName, suffix) { }
        
        // Here's an example with no parameters
        // Notice that there is no preposition or parameter name in the method name
        func fullName() { }

* Must return a value. If a valid value cannot be returned, a `NULL` object must be returned. In this case, however, you
  can't just return `null` though. You have to obey the rules in [Optional values](OptionalValues.md). In Swift, you can
  return `nil` because `nil` is type safe and follows the rules of optional values, but in other languages, you would
  return an optional wrapped object that must be unwrapped in order to see if it is `NULL` or not.)

* May not throw an exception in any case where all valid typed parameters are passed into the method. If the method
  calls methods that may throw an exception then those exceptions MUST be caught and a valid value returned every time
  and in every single situation where valid typed parameters are passed in.  
  
  >If the user passes invalid typed parameters into the function, then the behavior is undefined. Your code should never
  >try to handle situations where invalid parameter types are passed into the method. For example, if you have a *give
  >method* that takes 2 integers and the user passes in 2 strings instead, it's up to the language itself to figure out
  >what to do. Some languages will throw an exception and others will simply convert the values so that a value is
  >returned anyway. In any case, the user should NEVER pass invalid typed parameters into any method.

* May not modify the state of the object at all, ever. This means that *give methods* may not change property values,
  load data from an external source, or save data to an external source. They also may not modify any child objects or
  dependencies. The purpose of give methods is simply to compute and return data based on the current state of the
  object.
  
There are also certain special *give methods* that start with the word `_loaded`. These methods are reserved for lazy
loading properties and must be protected. The purpose of these methods are to load the value of a lazy property. For
more information regarding lazy properties, see [Properties](Properties.md)
  
**Give method summary**

* **Naming convention** - Must start with a noun, followed by a preposition, then the name of the first parameter. In the
  case that give methods must be used in place of computed properties, the name must be a noun. The noun in the name
  should represent the return value of the method.

* **Parameters** - Must take 1 or more parameters in most cases. In the case that give methods must be used in place of
  computed properties, give methods may take no parameters.
  
* **Return value** - Must return a valid value or a null wrapped object if a valid value cannot be returned.

* **Throws** - Must never throw an exception and must catch all exceptions if the method calls other methods that may
  throw an exception.
  
* **Call** - May call other *give methods* or *validate methods*. May not call any other methods.

* **State** - May not modify object state.
  
# Validate methods

The purpose of *validate methods* is to validate some type of data. They MUST take at least 1 parameter to validate
the value of. You should call *validate methods* from your init methods before setting the values of properties as well
as from any method that updates the state of the object. *Validate methods* are here to make sure that your objects are
ALWAYS in a valid state. Here are the rules for *validate methods*

* Must start with the word `validate` (or `_validate` if protected) and end with the name of the first parameter. In
  most cases, the word `validate` and the name of the first parameter *is* the method name, but it is acceptable to put
  other words in between in order to make the name descriptive of what it actually validates. Here are some examples:
  
        // WRONG
        // No parameters. All validate methods must take at least 1 parameter
        func validateName() { }
        
        // WRONG
        // Does not end with the name of the first parameter
        func validateUser(name) { }
        
        // CORRECT
        function validateUserName(userName) { }
        
        // ALSO CORRECT
        // Notice, there are other words to make the function more descriptive
        // This function validates the email address for a given user id
        function validateEmailForUserId(userId) { }

* Validate methods MUST take at least 1 parameter, no exceptions. There is no point in validating current state of an
  object because it should always be in a valid state.
  
* Validate methods must NEVER return a value, no exceptions. If you need a value, returned, you should be looking at
  [give methods](#give-methods) instead. If the method returns, then the value is valid. If the value is invalid, your
  method must throw an exception.
  
* Validate methods MUST throw an exception if the input cannot be validated or is invalid for any reason. The exception
  should (but doesn't have to) contain an error message that explains why validation failed. This error message can be
  extracted in order to be logged or to be displayed to the user if required.

Here is a simple example of a *validate method* written in pseudo code:

        // Sample validate method that validates the length of a name doesn't exceed 60 characters
        func _validateLengthOfName(name) {
            
            // If the length exceeds 60 characters, throw an exception
            if name.length > 60 {
                throw ValueExceptionWithMessage("The length of a name cannot exceed 60 characters")
            }
            
            // If we get here, it is validated, no need to return anything
        }

**Validate method summary**

* **Naming convention** - Must start with `validate` or `_validate` and end with the name of the first parameter. Other
  words are permitted in between to make the name more descriptive.
  
* **Parameters** - Must take at least 1 parameter.

* **Return value** - Must never return a value.

* **Throws** - May throw an exception if the value couldn't be validated.

* **Call** - May call *give methods* or other *validate methods*. May not call any other methods.

* **State** - May not modify object state.

# Do methods

The purpose of *do methods* is to modify the state of the object. They can either modify properties, load data from an
external source, or even save data to an external source. *Do methods* never return a value and may throw an exception
of the operation fails.

The rules for do methods are pretty simple. They must start with the word `do` or `_do` if protected and must end with
the name of the first parameter if there is one. They must never return a value either. *Do methods* are the *engine
and gears* of your class. Call these to *do* something. If you want to load data or save data to an external source, use
a *do method*. If you want to update the value of a property, use a *do method*.

By the way, you should never set a property directly from the outside. You should always use a *do method* to update a
property if it should be able to be updated.

Here is a simple example of a *do method* that updates the `name` property of the object:

        // Sample do method that updates the name property
        func doUpdateName(name) {
        
            // Validate the name first
            self._validateLengthOfName(name)
            
            // We are ok, set the value
            self.name = name
        }

**Do method summary**

* **Naming convention** - Must start with `do` or `_do` and end with the name of the first parameter if there is one.
  Other words are permitted in between to make the name more descriptive of what it does.
  
* **Parameters** - May take 0 or more parameters.

* **Return value** - Must never return a value

* **Throws** - May throw an exception if the operation could not be completed.

* **Call** - May call any other method except *init methods*

* **State** - May modify object state.

# On methods

*On methods* are just like *do methods*. They follow all the same rules as *do methods* except that they MUST be
protected and the name must start with `_on` instead of `do` or `_do`.

*On methods* are used as event handlers and are meant to be called when an event is triggered. For more information
about event handling see [Event handling](EventHandling.md)

**On method summary**

* **Naming convention** - Must start with `_on` and end with the name of the first parameter if there is one.
  Other words are permitted in between to make the name more descriptive of what it does.

* **Parameters** - May take 0 or more parameters.

* **Return value** - Must never return a value

* **Throws** - May throw an exception if the operation could not be completed.

* **Call** - May call any other method except *init methods*

* **State** - May modify object state.
