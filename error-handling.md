# Error Handling

## Resources

- [Returning status codes from business layer](https://codereview.stackexchange.com/questions/63049/returning-status-codes-from-business-layer)
- [Exceptions for flow control in C#](https://enterprisecraftsmanship.com/posts/exceptions-for-flow-control/)
- [Functional C#: Handling failures, input errors](https://enterprisecraftsmanship.com/posts/functional-c-handling-failures-input-errors/)
- [Error codes or Exceptions? Why is Reliable Software so Hard?](http://damienkatz.net/2006/04/error_code_vs_e.html)
- [Advanced error handling techniques](https://enterprisecraftsmanship.com/posts/advanced-error-handling-techniques/)

## Types of errors

- There are effectively two types of errors

  - Errors that you expect might happen
  - Errors that you don't expect at all
- Errors you expect typically are a result of:

  - Your application logic encountering a state that causes early exit of the current code execution:
    - Validation
    - Entities not found
    - Business logic violations
  - Other types
    - Intermittent issues with external API connectivity
    - SDK issues (out of bounds, format exceptions, etc)

## How errors are propagated

- Errors can be handled in two ways

  - Returning error codes
  - Throwing exceptions

- There is no cut and dry rule about when to use which, but there are lots of opinions.

- Some people argue:

  - Exceptions are better suited for unexpected errors.
  - Error codes are better for expected errors.

- The argument seems to gravitate around what the concept of "exceptional" means

  - > An exceptional situation, by definition, is unknown upfront. [Source](https://enterprisecraftsmanship.com/posts/exceptions-for-flow-control/#comment-4859422960)


## How errors can be handled

### Simply ignore the errors

- Simple workflows that usually aren't "transactional" in nature

### We have to unwind

- Typically for workflows that are transactional in nature.

- Consider an error in a workflow where previous steps have mutated state.

- When that error occurs, all the previous steps that lead to this error need to be reversed

- State needs to be restored to it's previous state.

- Sometimes this is easy, if the transaction is completely in-process, we can use the language's or frameworks "transaction" constructs.

  - Such us database transactions

- If the transaction is distributed

- Some nice points in *"Reverse the Flow of Time" Error Handling* [here](http://damienkatz.net/2006/04/error_code_vs_e.html).

  - > This is the real world, where things get screwed up in a big way because of the unexpected, the unknown, and going back is just as hard as going forward. We can't escape this in the real world, building a deck always has the possibility of being a huge disaster.

## Characteristics of Exceptions

- **Exceptions halt code execution**
  - Once thrown, the code block exits
  - Exception will bubble all the way up the call stack till something handles it.
  - If nothing handles it, the app will crash.

## Characteristics of Error Codes
- **An error code doesn't halt code execution.**
  - Code block returns the error code.
  - Caller is responsible then and there to read and handle the error code.
- **An error code can be ignored**
  - If error code is ignored, executable code can continue running in an invalid state.
- **Error codes add extra burden to developers**
  - Responses from methods now have potentially many cases to handle.
  - It's easier to expect the happy flow for the return type

## Exceptions pro and cons

### Benefits of exceptions

- **If the programmer doesn't catch the exception, it will bubble up the call stack.**
  - At some point you will know something wrong happen.
- **Error handling is "opt in"**
  - If you know there is an error you want to handle, you can catch it.
  - Otherwise, you can rest assured that if something does go wrong, it will be captured at the very top of the call stack
    - As long as you have global exception handling of course.

### Disadvantages of exceptions

- **Method signature is not explicit about what can go wrong.**
  - You have to pierce the veil of the black box to know what could go wrong
  - Or read the documentation
- **Empty try/catch blocks can swallow errors**
  - Like error codes being ignored.
- **They can be used like goto statements.**
  - Throw an exception one place
  - All of a sudden end up in another place somewhere in the callstack
  - You can't know where that might be unless you read through the upstream source code
  - Makes rationalising program execution harder.
  - Though in reality, the seriousness of this issue really depends on:
    - How many layers of code there are.
    - How often there are errors thrown in your code that must be handled.
    - Very often, we do want to exit code execution.
- **Error handling that leads to nested try/catch statements is less readable**
  - See Plan B Error handling [here](http://damienkatz.net/2006/04/error_code_vs_e.html).

## Error code pro and cons

### Advantages of error codes

- Method signatures are explicit in what can go wrong.

### Disadvantages of error codes

- **You have to react to error codes in every layer up the call stack**
- **Error codes might be misinterpreted and handled incorrectly**
  - Leading to more bugs
- **Error code handling requires discipline**
  - The more things to handle every time you call a method, the more painful coding becomes.
  - Pain leads avoiding the thing that causes you the pain.
- **Code _may_ become more verbose, less readable and harder to follow**
  - Error handling logic now required everywhere
    - Possibly increasing cyclomatic complexity
  - Can be mitigated by using functional programming concepts
  - A good argument regarding this point [here](https://enterprisecraftsmanship.com/posts/exceptions-for-flow-control/#comment-4862901150).
- **Changes to code _may_ cause changes to propogate up the call stack layers**

## Pointers for handling exceptions

- **Catch exceptions at the lowest level possible**
  
  - If you expect to need to handle a database exception, catch it in the layer that calls the database.
  - Avoid handling exceptions away from the call site that throws the exception
  - Though, a central exception handler is excluded from this rule
  
- **Create specific types of exceptions**

  - Catching exceptions that have semantic meaning in your code:
    - Validation exception
    - TODO: Elaborate on this more

- **Don't wrap code in generic try/catch handlers**

  - ```csharp
    try
    {
       // do something
    }
    catch(Exception e)
    {
       // we could never correctly handle every possible kind of exception
       // if we don't rethrow, there's a chance the exception is lost and a bug cannot be found
    }
    ```

  - Catching the generic base class exception is a code smell.

  - It will catch any exception, including exceptions that really should cause the current operation to be discarded immediately.

- **Use using blocks for resource clean up.**

  - Don't wrap your resource dependent code in try/catch handlers just to clean up resources if something goes wrong.
  - The language has you covered

## Personal conclusions

- Error code handling is great for situations where you need some very careful error handling.
  - However, forcing your entire codebase to have to handle error codes through each layer does not lead to a great developer experience.
  - Subsequently can lead to bugs where errors don't get handled at all
- Exceptions are best for when you don't explicitly need to handle errors that occur in downstream code
  - Though, in the application layer, exceptions explicitly defined for the application should be used.
- Exceptions are also great for when you don't want to force consumers of your method to have to handle errors, but if they choose to, they can (with a try/catch on a very explicit exception type)