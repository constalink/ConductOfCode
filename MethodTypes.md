# Method Types

Class methods are powerful and can do many things like take many parameters, return a value, throw an exception, modify
object properties, validate data, load and save data, and so much more. The problem with this is that under normal
circumstances, it's impossible to easily tell what a method will do when called. For this reason, we have created a set
of rules and restrictions for your methods. This includes what they are allowed to do and how they should be named and
called.

In general, every single method that you add to your classes should be categorized as one of the method types outlined
here. Each type of method has different requirements, restrictions, etc.

## Constructor methods

Constructor methods are called when an object instance is created. Most languages have a single constructor that allows
you to set default values for your properties. Here are the rules for constructor methods.

* Constructors may not take any parameters, ever.

* The only thing you can do in a constructor is set default values for properties, nothing more.

* Object initialization and validation should never be performed in the constructor method. That job is left to init
  methods instead.

## Init methods

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