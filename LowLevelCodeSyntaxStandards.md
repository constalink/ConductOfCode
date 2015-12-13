# Low Level Code Syntax Standards

The rules in this section are generic and are a general guide for your programming. It is possible that a given
language may not be able to follow all these standards. As such, you should follow these rules *if you can*, otherwise
you can. We've also included some language specific rules below. The rules in the language specific section take
priority over 

## White space and line endings

* Always indent using the **TAB** character  
  
* All files MUST use `\n` unix line endings, and not `\r\n` windows line endings  
  
* All files MUST end in a single `\n` new line character  
  
* There must never be any white space at the end of any line. This includes comment lines  
  
* Use a single space between operators. This includes assignment operators as well as comparison and other types of
operators.  
  
  * **WRONG** `intValue=0`
  * Instead use: `intValue = 0`  
  
  * **WRONG** `sumValue = 1+2`
  * Instead use: `sumValue = 1 + 2`
  
  * Don't use multiple spaces for alignment with other lines. Some argue that it promotes readability.... but the cost
    is that any potential parser, linter, checker, etc must account for multiple spaces instead of a single space.
    
  * **WRONG**
    ```
    intValue      = 2
    myStringValue = "Hello world"
    ```
  * Instead use:
    ```
    intValue = 2
    myStringValue = "Hello world"
    ```
  
* Function calls should be called with no space between the function name and opening parenthesis
  
  * **WRONG** `returnValue = funcName (param1)`
  * Instead use: `returnValue = funcName(param1)`

## Line length and wrapping

* In general line lengths should not exceed 120 characters.  
  
* Lines containing longer function names, variable declarations, etc are allowed to exceed this length, but if the
  language allows a method of keeping the line under 120 characters, it should be used. For example: If you have a 
  function with many *long named* parameters, you may be able to put those parameters on their own separate lines.  
  
  Instead of this:
  ```
  func (param1, param2, param3, param4) {
    // Some code
  }
  ```
  
  Use this:
  ```
  func (
    param1,
    param2,
    param3,
    param4
  ) {
    // Some code
  }
  ```
  
* Complex conditions should not simply take up the entire line  
  
  Don't do this:
  ```
  if ((condition1 && (!condition2 || condition3) || !(condition4 && condition5)) || condition6) {
    // Some code
  }
  ```
  
  Instead, you should break these up so that they are easier to read and understand
  ```
  combinedCondition1 = (!condition2 || condition3)
  combinedCondition2 = !(condition4 && condition5)
  combinedCondition3 = (condition1 && combinedCondition1 || combinedCondition2)
  
  if (combinedCondition3 || condition6) {
    // Some code
  }
  ```
  
* Comments that stretch over the 120 character length should be broken up into multiple comments or should be
  multi-line comments.
  
* In general, your code should be easy for others to read and understand. They shouldn't have to figure out what you
  are trying to do. If someone has to spend more than a few seconds to figure out what your intent is, you are doing
  it wrong. The 120 character length is put in place to encourage programmers to write simple, easy to understand code,
  nothing more. Therefore, this rule is not a deal breaker, but the you should really put some effort to stick to this
  rule for the most part.


## Control structures
  
* Always place opening brackets on the same line as the condition code and not on it's own line  
  
  * **WRONG** 
    ```
    if boolValue
    {
      // Some statements
    }
    ```
  
  * Instead use:
    ```
    if boolValue {
      // Some statements
    }
    ```


## Semicolons

* Never use semi colons unless the language requires them


## Comments

* Comments should never be on the same line as any code. Comments should always be on their own line and should
  be above the component that they refer to in general. (The exception is Python Doc Blocks which are below the 
  function definition)
  
* If there is still some work to do on something, you can use a *TODO* comment. Simply start any comment with `TODO:`
  in order to create a *TODO* comment. These types of comments let other developers know where work still needs to be
  done or where improvements can be made.
