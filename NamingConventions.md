# Naming Conventions

The rules in this section are generic and are a general guide for your programming. It is possible that a given
language may not be able to follow all these standards. As such, you should follow these rules *if you can*, otherwise
it's up to you to figure out what works best. All examples below are not written in any particular language, as such
you can treat the code as a sort of pseudo code. Each language may differ slightly but the code should help you
*get the hint*.

There may also be language specific rules below. Any rule that is specific to any language takes priority over any
general rule.

Generally, all your names should be descriptive and should represent *what it is*

# Convention Types

* **Camel case** This type starts with a lower case letter and follows conventional camel case conventions where the
  first letter of each word is capitalized
  
  *Example* `camelCaseName`
  
* **Capital camel case** This type follows the same conventions as *Camel Case* except that names start with a capital
  letter instead of a lower case letter
  
  *Example* `CapitalCamelCaseName`

* **All caps** This type requires all letters to be capital letters and each word to be separated with an underscore.
  
  *Example* `ALL_CAPS_NAME`

## Folder names

Folder names in your projects should follow the capital camel case convention. This means that they They should start
with a capital letter. Special folders that require different naming conventions are acceptable, but the program, os,
etc MUST require different naming conventions. For example, you might have a folder named `.ssh` that holds ssh keys.

## File names

File names for classes MUST follow the capital camel case convention and must have the same base name as the class 
itself. For example, a python class file for the `Date` class must be named `Date.py`.

Other files such as text files, json files, sql files, etc should follow the camel camel case convention. Special files
that require different naming conventions are acceptable, but the program, os, etc MUST require different naming
conventions. For example, you might have a file named `authorized_keys` that holds public ssh keys, or perhaps a file
named `__init__.py` as required by python.

## Global scope

Any global scope variables MUST follow the all caps convention with underscores separating each word. Although global
scope is highly discouraged, it may be useful in rare situations. If you must use global scope, use `ALL_CAPS`

        // WRONG
        GlobalInt = 10
        
        // CORRECT
        GLOBAL_INT = 10

## Class names

All class names must use the capital camel case convention. No exceptions.

## Function and method names

Method names must use the camel case convention. The only exception to this rule is when overriding parent methods
that are part of some standard library or perhaps a third party library that you are using in the project. If you are
overriding a parent method that does not follow the standard, and you have control over the naming of that parent
method, you must rename the parent method so that it follows the correct convention.

Also, methods that are not meant to be called from outside the class should start with an underscore.

        // Define a class
        class MyClass {
        
            // WRONG
            func my_custom_function() { }
            
            // CORRECT
            func myCustomFunction() { }
            
            // CORRECT
            func _myPrivateFunction() { }
        }

Static / class methods follow the same naming convention as instance methods. Furthermore, each type of method has
different restrictions as far as how it can be named. For more information on those restrictions, check out
[Method Types](MethodTypes.md)

## Function parameter names

Function parameter names must use the camel case convention. The only exception to this rule is when overriding
parent methods that are part of some standard library or perhaps a third party library that you are using in the
project, and the signature of the method must match in order to work correctly. You'll find that this applies to
swift and python and it may apply to other languages as well.
  
        // Define a class
        class MyClass {
        
            // WRONG
            func myCustomFunc(first_param, second_param) { }
            
            // CORRECT
            func myCustomFunc(firstParam, secondParam) { }
        }

## Local variable names

All local variable names must use the camel case convention. No exceptions.

        // WRONG
        my_int_var = 1
        
        // CORRECT
        myIntVar = 1

## Property names

All properties of a class must use the camel case convention. The only exceptions to this rule is when overriding
parent properties that follow a different convention, or when a property is specially named due to the nature of the
os, language, or platform. Also, properties that are not meant to be seen from outside the class should start with
an underscore.

        // Define a class
        class MyClass {
        
            // WRONG
            property my_property
            
            // CORRECT
            property myProperty
            
            // CORRECT
            property _myPrivateProperty
        }

Static / class properties follow the same naming convention as instance properties.

## Enum names

All enums should start with the letters `En` and follow the capital camel case convention. They should be named for the
singular version of one of the enum cases, not the plural version.

        // Correct examples
        enum EnFileKind
        
        enum EnOsKind
        
        enum EnColor
        
        // WRONG examples
        // Doesn't start with En
        enum FileKind
        
        // Isn't the singular version
        enum EnColors

