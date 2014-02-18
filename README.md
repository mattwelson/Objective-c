# Objective-c

This is a quick write up on how to start learning objective-c. 

## Language features
Objective-c is a weird looking language. It's an object oriented declarative language that uses dynamic typing and dynamic binding. Here's a list of a few features that may take some getting used to. 
### Methods
Methods can also be referred to as message passing. Instead of invoking a method on another object, you are asking it nicely to please do it and let you know what happens. This is similar to a language called Small Talk (this won't be news to you if you've met Richard O'Keefe)
#### Declaration
The following examples are methods that belong to the [NSArray class](https://developer.apple.com/library/mac/documentation/Cocoa/Reference/Foundation/Classes/NSArray_Class/NSArray.html)
##### A method declaration with no parameters:
```objective-c
- (id)firstObject
```
The hyphen means it is an instance method (non-static for you Java nuts) where a plus would mean static.
The id in brackets is the return type and firstObject is the method name. This would be followed by a semi colon to declare the method or set of curly braces with the method body.
(id means the pointer that all NSObjects must have)
##### A method declaration with one parameter:
```objective-c
- (id)objectAtIndex:(NSUInteger)index
```
A colon indicates that a parameter is to follow, of type NSUInteger (an unsigned long) with the parameter name index. Simple. Wait for the next one.
##### A method with more than one parameter:
```objective-c
- (NSUInteger)indexOfObject:(id)anObject inRange:(NSRange)range
```


#### Call

## Variables
### Instance
### Macros
### Constants
### Properties

## Misc
### Blocks
### Structs
Every now and again you'll run into a struct. This is exactly the same as it is c. They are used to represent things like the frame of anything that is rendered onto the screen, representing the x and y coordinates as well as the width and height.

##Hello, world program
1. Fire up Xcode, it is an OS X only application so will need to run on a mac or in virtual machine
2. Click create a new Xcode project, we'll start with the Command Line Tool option, under OS X as a template
3. Set a project name (HelloWorld) and fill in the company identifier (Practice), set the type drop down to Foundation
4. Save to a sensible place
5. Embarrassingly, the template already says hello world. Switch to main.m to find it
    ```objective-c
    #import <Foundation/Foundation.h>
    
    int main(int argc, const char * argv[])
    {
    
        @autoreleasepool {
            
            // insert code here...
            NSLog(@"Hello, World!");
            
        }
        return 0;
    }
    ```
    The dot m extension is where you place all the executable code, while public header files use a dot h extension, more on that later!
6. Click the play arrow in the top left to build and run the solution, or press command + r
When the solution runs, the output pane will pop up from the bottom of the xcode window. It will say the date and some other stuff on the same line as "Hello, World!", then an exit code of 0 - which we know means success from c.

###Things to note:
- @autoreleasepool refers to automatic reference counting (ARC), this is the way that the foundation framework manages memory. Anything that is executed in its scope will take care of itself, with one exception.
- The @ infront of a string makes it an NSString, the foundation framework version of a string. [It's class reference can be found here](https://developer.apple.com/library/mac/documentation/Cocoa/Reference/Foundation/Classes/NSString_Class/Reference/NSString.html)
- NSLog is what printed out the date (also a line break) but you could use printf, as objective-c extends c. Printf does not work with NSStrings, so you would need to remove the at sign, and append a new line character. 
```objective-c
  printf("Hello, World\n");
```

##Expanding the program
Let's start writing some code of our own. The hello world example was stolen by Apple, so let's write a program that will create an array of different greetings, then traverse over it to print each one. 
1. I just deleted the old NSLog line so I had nothing between the @autoreleasepool flags
2. The collection we will use is called an [NSArray](https://developer.apple.com/library/mac/documentation/Cocoa/Reference/Foundation/Classes/NSArray_Class/NSArray.html) and there are at least 3 ways to initialise one. 
  1. The general way
```objective-c
    NSArray *greetings = [[NSArray alloc] init];
```
  2. A convenience constructor, lots of objects have these
```objective-c
    NSArray *greetings = [NSArray arrayWithObjects:@"Hello", @"Bonjour", @"Guten tag"];
```
  3. The literal way
```objective-c
    NSArray *greetings = @[@"Hello", @"Bonjour", @"Guten tag"];
```
Here the square brackets represent the array literal (like quotation marks represent string literals) and not method calls.
