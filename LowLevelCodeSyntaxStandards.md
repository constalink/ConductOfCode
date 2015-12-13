# Low Level Code Syntax Standards

The rules in this section are generic and are a general guide for your programming. It is possible that a given
language may not be able to follow all these standards. As such, you should follow these rules *if you can*, otherwise
it's up to you to figure out what works best. All examples below are not written in any particular language, as such
you can treat the code as a sort of pseudo code. Each language may differ slightly but the code should help you
*get the hint*.

There may also be language specific rules below. Any rule that is specific to any language takes priority over any
general rule.

## White space and line endings

* Always indent using the **TAB** character  
  
* All files MUST use `\n` unix line endings, and not `\r\n` windows line endings  
  
* All files MUST end in a single `\n` new line character  
  
* There must never be any white space at the end of any line. This includes comment lines  
  
* Items in lists, dictionaries, tuples, function parameters, and other sets or items that are iterable should be
separated with a space between each item. If necessary, you can also put each item on it's own line. If all items are on
the same line, then there should be no space between the opening bracket or parenthesis.
  
        // WRONG
        myList = [item1,item2,item3]
        
        // Use spaces between items instead
        // CORRECT
        myList = [item1, item2, item3]
        
        // Don't use a space between the opening bracket and the first item
        // And don't use a space between the last item and the closing bracket
        // WRONG
        myList = [ item1, item2, item3 ]
        
        // The same thing goes for function parameters or function calls
        // WRONG
        returnValue = functionCall( param1, param2 )
        
        // CORRECT
        returnValue = functionCall(param1, param2)
        
        // Again, the same thing goes for dictionaries
        // CORRECT
        myDict = {name1: value1, name2: value2}
        
        // You can also have each item on it's own line if the list exceeds the max line length
        // or if it's just easier to read
        // CORRECT
        myList = [
            item1,
            item2,
            item3
        ]
        
        // You can even put comments inside your lists if you have each item on it's own line
        // CORRECT
        myList = [
        
            // Item 1
            item1,
            
            // Item 2
            item2,
            
            // Item 3
            item3
        ]
  
* Use a single space between operators. This includes assignment operators as well as comparison and other types of
operators.  
  
        // WRONG
        intValue=0
        
        // CORRECT
        intValue = 0
        
        // WRONG
        sumValue = 1+2 
        
        // CORRECT
        sumValue = 1 + 2 
  
  Don't use multiple spaces for alignment with other lines. Some argue that it promotes readability.... but the cost
  is that any potential parser, linter, checker, etc must account for multiple spaces instead of a single space.  
        
        // WRONG  
        intValue      = 2
        myStringValue = "Hello world"
        
        // CORRECT
        intValue = 2
        myStringValue = "Hello world"
  
* Function calls should be called with no space between the function name and opening parenthesis  
  
        // WRONG
        returnValue = funcName (param1)
        
        // CORRECT
        returnValue = funcName(param1)
  
  
## Line length and wrapping
  
* In general line lengths should not exceed 120 characters.  
  
* Lines containing longer function names, variable declarations, etc are allowed to exceed this length, but if the
  language allows a method of keeping the line under 120 characters, it should be used. For example: If you have a 
  function with many *long named* parameters, you may be able to put those parameters on their own separate lines.  
  
        // Instead of this:
        func (param1, param2, param3, param4) {
            // Some code
        } 
        
        // You could use this:
        func (
            param1,
            param2,
            param3,
            param4
        ) {
            // Some code
        }
  
* Complex conditions should not simply take up the entire line  
  
        // Don't do this:
        if ((condition1 && (!condition2 || condition3) || !(condition4 && condition5)) || condition6) {
            // Some code
        }
        
        // Instead, you should break these up so that they are easier to read and understand  
        combinedCondition1 = (!condition2 || condition3)
        combinedCondition2 = !(condition4 && condition5)
        combinedCondition3 = (condition1 && combinedCondition1 || combinedCondition2)
        
        // Now, the complex condition becomes a simple OR condition
        if (combinedCondition3 || condition6) {
            // Some code
        }
  
* Comments that stretch over the 120 character length should be broken up into multiple comments or should be
  multi-line comments.  
  
* In general, your code should be easy for others to read and understand. They shouldn't have to figure out what you
  are trying to do. If someone has to spend more than a few seconds to figure out what your intent is, you are doing
  it wrong. The 120 character length is put in place to encourage programmers to write simple, easy to understand code,
  nothing more. Therefore, this rule is not a deal breaker, but the you should really put some effort to stick to this
  rule for the most part.  
  
  
## Control structures
  
* Always place opening braces on the same line as the condition code and not on it's own line  

        // WRONG
        if boolValue
        {
            // Some statements
        }
        
        // CORRECT
        if boolValue {
            // Some statements
        }

* Multiple else if's or a final else may be put on the same line as the last closing brace or on it's own line, but
the opening brace for each of them must not be on a new line. It's pretty flexible as the only real requirement is
that you don't put opening braces on a new line. You can even place comments between conditions.

        // WRONG
        if boolValue {
            // Some statements
        } 
        
        else
        {
            // Some statements
        }
        
        // CORRECT
        if boolValue {
            // Some statements
        } else {
            // Some statements
        }
        
        // ALSO CORRECT
        if boolValue {
            // Some statements
        }
        
        else {
            // Some statements
        }
        
        // You can even comment each condition
        // CORRECT
        if boolValue {
            // Some statements
        }
        
        // Comment for the next section
        else if boolValue2 {
            // Some statements
        }
        
        // Otherwise, do this
        else {
            // Some statements
        }
  
## Semicolons
  
* Never use semi colons unless the language **requires** them. This means don't use semicolons in JavaScript, Python,
Swift, etc... Since PHP requires them, you must use them in PHP
  
  
## Comments
  
* Comments should never be on the same line as any code. Comments should always be on their own line and should
  be above the component that they refer to in general. (The exception is Python Doc Blocks which are below the 
  function definition)  
  
* If there is still some work to do on something, you can use a *TODO* comment. Simply start any comment with `TODO:`
  in order to create a *TODO* comment. These types of comments let other developers know where work still needs to be
  done or where improvements can be made.  
  
        // This is not a todo comment
        
        // TODO: This is a todo comment
        
        // TODO:Don't do this... separate the comment and colon with a space
        
        // TODO: This is the correct way to make a todo comment


## Quotes

* Always use double quotes for strings unless the language requires single quotes.

        // WRONG
        myStringValue = 'Hello world'
        
        // CORRECT
        myStringValue = "Hello world"
