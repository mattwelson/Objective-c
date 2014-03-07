# Objective-c

This is a quick write up on how to start learning objective-c. There's lots of great guides for working with cocoa-touch which sideline the language as much as possible, this crash course should help you get your bearings and then point you towards a few great tutorials. Don't worry too much about taking everything in at once.

## Language features
Objective-c is a weird looking language. It's an [object oriented](http://en.wikipedia.org/wiki/Object-oriented_programming) [procedural language](http://en.wikipedia.org/wiki/Procedural_programming) that uses [message passing](http://en.wikipedia.org/wiki/Message_passing). Here's a list of a few features that may take some getting used to. 
### Methods
Methods can also be referred to as message passing. Methods are invoked in the form of ```[object method:parameter];```. Instead of invoking a method on another object, you are asking it nicely to please do it and let you know what happens. This is similar to a language called Smalltalk (this won't be news to you if you've met Richard O'Keefe). This is were dynamic binding comes into play, the compiler doesn't resolve what methods will be called, instead this is worked out at runtime. In practice this is similar to overriding methods in Java or other languages.
#### Declaration
The following examples are methods that belong to the [NSArray class](https://developer.apple.com/library/mac/documentation/Cocoa/Reference/Foundation/Classes/NSArray_Class/NSArray.html)
##### A method declaration with no parameters:
```objc
- (id)firstObject
```
The hyphen means it is an instance method (non-static for you Java nuts) where a plus would mean static.
The id in brackets is the return type and firstObject is the method name. This would be followed by a semi colon to declare the method or set of curly braces with the method body.
(id means the pointer that all NSObjects must have)
##### A method declaration with one parameter:
```objc
- (id)objectAtIndex:(NSUInteger)index
```
A colon indicates that a parameter is to follow, of type NSUInteger (an unsigned long) with the parameter name index. Parameters can objects, structs, blocks of code, method signatures (called selectors) or primitive types.
##### A method with more than one parameter:
```objc
- (NSUInteger)indexOfObject:(id)anObject inRange:(NSRange)range
- (NSUInteger)indexOfObject:(id)anObject:(NSRange)range //equivalent to above. 'inRange' is optional text which is useful for a readability perspective
```
Instead of the usual comma separated list of parameters, objective-c uses a much longer style. Methods instead use a space, a phrase and then a colon to seperate each parameter. Methods cannot be overloaded, but there is no need to - you can just change part of the name to reflect the difference. This pattern will work well for up to 4 parameters, after which you should consider passing in a dictionary, or switching to other object oriented design patterns.

#### Call
For the following examples, assume a succesfully initialised NSArray object named array. Creative.
##### No parameters
```objc
NSString *string = [array firstObject];
```
This example accesses the first object in the array, and assigns it to a new NSString.  Like c the * just means it is a pointer to an object. 
##### One parameter
```objc
NSString *string = [array objectAtIndex:1];
```
This accesses the second object in the array, and assigns it to a new NSString named string.
##### Lots of parameters
```objc
int index = [array indexOfObject:@"Hello" inRange:NSMakeRange(0, [array count]/2)];
```
This will look for the object in only the first half of the array. From the documentation I know that it is calling the isEqual method on each object, so it's looking at deep equality rather than shallow. For the range parameter I am using a c style method, which returns a range struct. This can all happen as objective-c is built on top of c, any c code is valid objective-c code. As a parameter to the c method I am using the count method of the array to get its length. 

## Variables
###Primitives
Booleans can be bool or BOOL and can be assigned values of YES, NO, true, false. YES and NO and BOOL are effectively just macros that mean yes == true and no == false and BOOL == bool, so using any combinations of these makes no difference whatsoever. 
```objc
BOOL b; 
bool b;
b = YES;
b = true;
b = NO;
b = false;
```
Ints are the same as every other language essentially. If you dont know how ints work by now, then it is amazing that you have made it to 3rd year.
```objc
int i = 7;
```
Doubles/floats - as above
```objc
double d = 3.1;
float f = 4.0;
[someObject someMethodThatTakesAFloatAsAnArgument:1.0f]; //The f suffix is commonly used to make sure a number is a float
```
Chars and unichars
```objc
char c = 'v';
unichar c = 'k';
```

### Instance
Instance variables are objects that you need to initialise through a class constructor.
In java this is done as follows:
```java
MyObject obj = new MyObject();
````
This is done in an entirely different way in objective-c and can take some getting used to. When an object is declared you must put a '*' symbol (pointer) to give the variable a pointer value. Generally to initialise an object you must allocate memory (dont worry) and call an initialisation method:
```objc
MyObject *obj; //declare object
obj = [[MyObject alloc] init]; //initialise object
MyObject *obj = [[MyObject alloc] init]; //declare and initialise object at once
```
The alloc and init methods are common across every subclass of NSObject (much like java's Object) so you are not required to write and manage memory yourself.
A common thing to do is to write some custom init methods which provide different initialisations. 

```objc
-(id)init{
    if(self = [super init]){
        //custom init code
    }
}

//custom init
-(id)initWithSomeVariableSetToSomething:(int)var{
    if(self = [super init]){
        self.theVariableToSet = var
    }
}
//call the above init method
MyObject *obj = [[MyObject alloc] initWithSomeVariableSetToSomething:8];

//example from one of my apps - loading and setting variables when unarchived
- (id)initWithCoder:(NSCoder *)coder {
    self = [self init];
    self.title = [coder decodeObjectForKey:@"title"];
    self.timeline = [coder decodeObjectForKey:@"timeline"];
    self.length = [coder decodeDoubleForKey:@"length"];
    self.colour = [coder decodeIntForKey:@"colour"];
    return self;
}
//the above init will get called when the following is called (more on archiving later)
MyObject *obj = [NSKeyedUnarchiver unarchiveObjectWithFile:filename];
```
If an object has a method with a + instead of - then you can call the method without needing to do the alloc and init
```objc
//return a String instance
+(id)initAString{
    return @"A string";
}
//return a SomeObject instance (in AClass.m)
+(id)initSomeObject{
    return [[SomeObject alloc] init];
}
//calling a class method to init an object
NSString *s = [AClass initAString];
SomeObject *o = [AClass initSomeObject];
```
One of the most common instance variables you will use will be NSString, the string type for Objective-c. NSString have a lot of class methods to choose from. And can be initialised in many ways.
```objc
NSString *s;
s = @"A string";
s = [NSString stringWithFormat:@"Insert an int %i and a float %f and another string %@", 1, 1.0f, @"Another String"];
s = [NSString stringWithString:@"Another string"];
NSLog(@"%@",s); //print s
```
#### nil vs NULL vs NSNull
*[NSHipster's take.](http://nshipster.com/nil/)*
In c NULL is the null pointer, when using objective-c you have a few more options. An uninitialised object is called nil, which is technically identical to NULL, but semantically different as it is only used for objective-c objects. NSNull is a singleton class (there can be only one) which can be used to represent nothing, but can go into collections, unlike nil or NULL. 
If you call a method on nil(say you forgot to initialise a variable), you do not get a null pointer exception, instead the message is quietly ignored. This can be extremely frustrating. For example setting the text property of a button before it is initialised will do absolutely nothing. 
*Take home message: Use nil for objective-c objects*

### Macros
Macros are very handy things, they are mostly used to make things easier on the programmer. 
```objc
//these helpful macros make colour creation a breeze
#define RGBA(r,g,b,a)				[UIColor colorWithRed:r/255.0f green:g/255.0f blue:b/255.0f alpha:a]
#define RGB(r,g,b)					RGBA(r, g, b, 1.0f)
//Check what the device is
#define IS_IPAD   (UI_USER_INTERFACE_IDIOM() == UIUserInterfaceIdiomPad)
if(IS_IPAD){
    //Do iPad code
}else{
    //You are on an non iPad device
}
```

Macros come from the c language, before your code is compiled the macro you have defined is substituted into anywhere you've used it in code. A macro may or may not take parameters.

Macros have a love/hate relationship with most developers. They're actually super easy to use but they're also super easy to introduce bugs with and they hide those bugs really well. I tend to use these as quick and nasty constants but this is largely discouraged. It is possible to use macros to hold global constants but this is widely discouraged, see constants below.
[Arguments against macros.](http://weblog.highorderbit.com/post/11656225202/appropriate-use-of-c-macros-for-objective-c-developers)

### Constants
Constants are little more fiddly to make global, but not by much. They aren't substituted into your code by the precompiler which gives them a few advantages and makes them a lot easier to debug. 

To make constants for a file:
```objc
NSString *const DefaultFont = @"Avenir-Book";
```

[Stack Overflow on global constants](http://stackoverflow.com/questions/538996/constants-in-objective-c)

### Defining a New Class
Creating your own class is done by adding a new file through Xcode. Each class that's created must be a subclass of something, with [NSObject](https://developer.apple.com/library/ios/documentation/Cocoa/Reference/Foundation/Classes/NSObject_Class/Reference/Reference.html) being the root of all (or nearly all) classes.

#### Person.h
To make a class that represents a person we could make the following interface which exposes any public properties and methods needed.
```objc
#import <Foundation/Foundation.h>
@class Country; // means your h file can reference another class, but doesn't import it (good practice)

@interface Person : NSObject // the class must inherit from something, NSObject in this case

@property NSString *firstName;
@property NSString *lastName;
@propert Country *countryOfOrigin;

+ (instancetype) personWithFirstName:(NSString *)firstName lastName:(NSString *)lastName fromCountry:(Country *)country;
    // an objective-c constructor, that intialises the object

- (NSString *) fullName; // an instance method to return a person's full name as an NSString

@end
```

#### Person.m
Then use the following implementation to implement each of the public methods and any private ones that are needed
```objc
#import "Person.h" // the interface we're conforming to

@implementation Person

// a private init method
- (instancetype) initWithFirstName:(NSString *)firstName lastName:(NSString *)lastName fromCountry:(Country *)country
{
    self = [super init]; 
    if (self){ // checks memory was allocated properly
        self.firstName = firstName;
        self.lastName = lastName;
        self.countryOfOrigin = country;
    }
    return self;
}

+ (instancetype) personWithFirstName:(NSString *)firstName lastName:(NSString *)lastName fromCountry:(Country *)country
{
    return [[Person alloc] initWithFirstName:firstName lastName:lastName fromCountry:country];
}

- (NSString *) fullName
{
    return [NSString stringWithFormat:@"%@ %@", self.firstName, self.lastName];
}

// The following is overriding a method of NSObject so is public without needing to be in the h file, the 
// equivalent of toString, but not used often
- (NSString *) description
{
    return [NSString stringWithFormat:@"%@, %@ from %@", self.lastName, self.firstName, self.countryOfOrigin]; 
}

@end
```

### Properties
Properties are awesome. Properties live in the .h file for a class.

#### Why aren't we mentioning synthesize?
Synthesize is dead. If you see it in a tutorial, find another one. It's a keyword that was removed mid 2012, which you'll note isn't that recent. 

What did it do? It used to create your properties getters and setters, which is now done automatically

## Misc
###Mutable vs Immutable
Most objective-c objects are mutable by default, but there are some which have differently named mutable/immutable classes. Immutable objects are essentially read-only once they are initialised, while Mutable objects are editable after they are initialised. 
    For example, suppose you initialise an array as in the above example:
```objc
NSArray *greetings = [NSArray arrayWithObjects:@"Hello", @"Bonjour", @"Guten tag"];
```
Then wanted to add an object or modify the index, you would need to create an entirely new NSArray to make the changes. However, by using an NSMutableArray it is as simple as:
    
```objc
NSMutableArray *greetings = [NSMutableArray arrayWithObjects:@"Hello", @"Bonjour", @"Guten tag"];
[greetings addObject:@"Konnichiwa"]; //append object to array
[greetings insertObject:@"Nihao" atIndex:2]; //insert object at index 2
```
    
If you wish to make an Immutable object from a Mutable object you can do this fairly easily
    
```objc
NSArray *immutableGreetings = greetings; //make an immutable version of greetings array
```
    
A list of the objects which have differently named mutable/immutable counterparts:
```objc
-NSMutableArray
-NSMutableDictionary
-NSMutableSet
-NSMutableIndexSet
-NSMutableCharacterSet
-NSMutableData
-NSMutableString
-NSMutableAttributedString
-NSMutableURLRequest
```
### Blocks
[Blocks are hard.](http://code.tutsplus.com/tutorials/understanding-objective-c-blocks--mobile-14319)

They are used for passing in *blocks* of code as an argument in several methods, you can use them to animate your views in an app, or to sort an array.

### Structs
Every now and again you'll run into a struct. This is exactly the same as it is c. They are used to represent things like the frame of anything that is rendered onto the screen, representing the x and y coordinates as well as the width and height.

##Hello, world program
1. Fire up Xcode, it is an OS X only application so will need to run on a mac or in virtual machine
2. Click create a new Xcode project, we'll start with the Command Line Tool option, under OS X as a template
3. Set a project name (HelloWorld) and fill in the company identifier (Practice), set the type drop down to Foundation
4. Save to a sensible place
5. Embarrassingly, the template already says hello world. Switch to main.m to find it

    ```objc
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
    
    *Important note:* When programming with cocoa you will not touch the main method. Instead you'll be customising objects that the frame work will instantiate and call certain methods on. This will look quite different but other tutorials will cover this.
6. Click the play arrow in the top left to build and run the solution, or press command + r
When the solution runs, the output pane will pop up from the bottom of the xcode window. It will say the date and some other stuff on the same line as "Hello, World!", then an exit code of 0 - which we know means success from c.

###Things to note:
- @autoreleasepool refers to automatic reference counting (ARC), this is the way that the foundation framework manages memory. Anything that is executed in its scope will take care of itself, with one exception.
- The @ infront of a string makes it an NSString, the foundation framework version of a string. [It's class reference can be found here](https://developer.apple.com/library/mac/documentation/Cocoa/Reference/Foundation/Classes/NSString_Class/Reference/NSString.html)
- NSLog is what printed out the date (also a line break) but you could use printf, as objective-c extends c. Printf does not work with NSStrings, so you would need to remove the at sign, and append a new line character. 
```objc
  printf("Hello, World\n");
```

##Expanding the program
Let's start writing some code of our own. The hello world example was stolen by Apple, so let's write a program that will create an array of different greetings, then traverse over it to print each one. 
1. I just deleted the old NSLog line so I had nothing between the @autoreleasepool flags
2. The collection we will use is called an [NSArray](https://developer.apple.com/library/mac/documentation/Cocoa/Reference/Foundation/Classes/NSArray_Class/NSArray.html) and there are at least 3 ways to initialise one. 
1. The general way    
```objc
        NSArray *greetings = [[NSArray alloc] initWithObjects:@"Hello", @"Bonjour", @"Guten tag"];
```
2. A convenience constructor, lots of objects have these    
```objc
        NSArray *greetings = [NSArray arrayWithObjects:@"Hello", @"Bonjour", @"Guten tag"];
```
3. The literal way    
```objc
        NSArray *greetings = @[@"Hello", @"Bonjour", @"Guten tag"];
```
Here the square brackets represent the array literal (like quotation marks represent string literals) and not method calls.
4. To iterate over our list and print each greeting you can use a foreach or a for loop (or a while loop if you're a maniac) as well as a few fancy enumerators.
    1. A handy old foreach loop, great when you don't care about index
```objc
        for (NSString *greeting in greetings)
        {
            NSLog(@"%@", greeting);
        }
```
or if you don't know or care about type you could use the generic type id
```objc
        for (id greeting in greetings)
        {
            NSLog(@"%@", greeting);
        }
```
5. A  familiar for loop. When written in full with each method that is being used being called explicitly:
```objc
        for (int i = 0; i < [greetings count]; i ++)
        {
            NSLog(@"%@", [greetings objectAtIndex:i]);
        }
```
In short hand:
```objc
        for (int i = 0; i < greetings.count; i ++)
        {
            NSLog(@"%@", greetings[i]);
        }
```
The short hand works because count is a property of an array, and the getter method for a property is called when you use dot notation. The objectAtIndex: method is called when we use the more conventional index notation, just for convenience. It's up to you which way you prefer, I'd suggest being consistent.

*A note on printing: You may have noticed I'm using NSLog differently now. This is how it should be used, the first argument is a format string using %@ symbol to represent another string, which is given as an argument. This prevents a warning in XCode (so there's less noise for your warnings) and allows you to create complex outputs by including multiple arguments in one long string.*

# The End.
And now you're ready to start looking at using Cocoa-touch. To begin your journey we've listed a few guides below. Feel free to add anything you'd like to know as a comment or issue here on github, or contact us anyway you know how. 

Remember building your first app should be fun, relatively simple and a great exercise in teamwork and software development. Don't forget to use source control, github offer 5 private repos to students for free.

## Matt's links:
1. Start with a basic app:
2. Learn to do it from scratch: [Hello world take 2](http://www.appcoda.com/hello-world-app-using-xcode-5-xib/)
3. Then a harder app: [Ray Wenderlich's bugs](http://www.raywenderlich.com/1797/ios-tutorial-how-to-create-a-simple-iphone-app-part-1)
4. 
5. Learn to love stack overflow: [You'll spend a long time on here](http://stackoverflow.com/questions/tagged/cocoa-touch), remember - most answers are wrong.
6. Learn to love source control: ... you're already on github, may as well look around.
7. Check out a style guide or two: [NY Times style guide](https://github.com/NYTimes/objective-c-style-guide)
8. Start learning pretty pro techniques: [Awesome weather app](http://www.raywenderlich.com/55384/ios-7-best-practices-part-1) - This app uses Cocoapods (community built modular code), [ReactiveCocoa](http://nshipster.com/reactivecocoa/) (allows classes to watch for changes in other classes with minimal coupling, see also: [KVO](http://nshipster.com/key-value-observing/)) and calls a web api. Fancy!
