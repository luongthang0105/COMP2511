# Context

Same functionalities appears a lot of places:

For example:

- An application may have a copy button or "Ctrl+C" shortcut to do copy, etc.
- The code of **copy** appears in different places, which will be very annoying if we need to modify.
- This application have a lot of commands which has the same problem.
- The application also wants to implement undo operations.

# How it works?

We may have 3 layers of objects here:

- An Invoker
- A set of Commands
- A Receiver

So an Invoker is going to store a command object. For example, a copy button stores the Copy command.

When an Invoker invokes the command, the command object will tell the receiver to do its job. In our text editor example, the Copy command will tell the text editor to do the copy operation.

# Implementation

Let's follow (this)[https://www.newthinktank.com/2012/09/command-design-pattern-tutorial/].
