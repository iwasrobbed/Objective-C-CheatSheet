## Objective-C Cheat Sheet

This is not meant to be a beginner's guide or a detailed discussion about Objective-C; it is meant to be a quick reference to common, high level topics.

* Read my [Swift](https://github.com/iwasrobbed/Swift-CheatSheet) cheatsheet as well (Swift will replace Objective-C for iOS apps).
* Support this project via Gratipay <a href="https://gratipay.com/iwasrobbed/"><img src="http://img.shields.io/gratipay/iwasrobbed.svg"></a>
* To download a PDF version of this, use [GitPrint.com](https://gitprint.com/iwasrobbed/Objective-C-CheatSheet/blob/master/README.md?download)

**Note**: I wrote this over a stretch of 3 days, so it could probably use some editing and clarifications where necessary. Please feel free to edit this document to update or improve upon it, making sure to keep with the general formatting of the document.  The list of contributors can be [found here](https://github.com/iwasrobbed/Objective-C-CheatSheet/graphs/contributors).

For advanced Objective-C topics, these three sites are updated weekly:
* http://www.objc.io
* http://nshipster.com
* https://iosdevweekly.com

If something isn't mentioned here, it's probably covered in detail in one of these:

* [Swift, Apple's new language to replace Objective-C](https://developer.apple.com/library/prerelease/ios/documentation/Swift/Conceptual/Swift_Programming_Language)
* [NSHipster's "Fake" book aka concise usage examples of common Objective-C tasks](https://gumroad.com/l/the-nshipster-fake-book)
* [The Objective-C Programming Language](http://developer.apple.com/library/mac/#documentation/Cocoa/Conceptual/ObjectiveC/Introduction/introObjectiveC.html)
* [Coding Guidelines for Cocoa](https://developer.apple.com/library/mac/#documentation/Cocoa/Conceptual/CodingGuidelines/CodingGuidelines.html)
* [iOS App Programming Guide](http://developer.apple.com/library/ios/#documentation/iphone/conceptual/iphoneosprogrammingguide/Introduction/Introduction.html)
* [Cocoa Encyclopedia](https://developer.apple.com/library/mac/documentation/general/conceptual/CocoaEncyclopedia/)

### Table of Contents

* [Commenting](#commenting)
* [Data Types](#data-types)
* [Constants](#constants)
* [Operators](#operators)
* [Declaring Classes](#declaring-classes)
* [Preprocessor Directives](#preprocessor-directives)
* [Compiler Directives](#compiler-directives)
* [Literals](#literals)
* [Methods](#methods)
* [Properties and Variables](#properties-and-variables)
* [Naming Conventions](#naming-conventions)
* [Blocks](#blocks)
* [Control Statements](#control-statements)
* [Enumeration](#enumeration)
* [Extending Classes](#extending-classes)
* [Error Handling](#error-handling)
* [Passing Information](#passing-information)
* [User Defaults](#user-defaults)
* [Common Patterns](#common-patterns)

## Commenting

Comments should be used to organize code and to provide extra information for future refactoring or for other developers who might be reading your code.  Comments are ignored by the compiler so they do not increase the compiled program size.

Two ways of commenting:

```objC
// This is an inline comment

/* This is a block comment
   and it can span multiple lines. */

// You can also use it to comment out code
/*
- (SomeOtherClass *)doWork
{
    // Implement this
}
*/
```

Using `pragma` to organize your code:
```objC
#pragma mark - Use pragma mark to logically organize your code

// Declare some methods or variables here

#pragma mark - They also show up nicely in the properties/methods list in Xcode

// Declare some more methods or variables here
```

[Back to top](#objective-c-cheat-sheet)

## Data Types

### Size

Permissible sizes of data types are determined by how many bytes of memory are allocated for that specific type and whether it's a 32-bit or 64-bit environment.  In a 32-bit environment, `long` is given 4 bytes, which equates to a total range of `2^(4*8)` (with 8 bits in a byte) or `4294967295`.  In a 64-bit environment, `long` is given 8 bytes, which equates to `2^(8*8)` or `1.84467440737096e19`.

For a complete guide to 64-bit changes, please [see the transition document](https://developer.apple.com/library/mac/documentation/Darwin/Conceptual/64bitPorting/transition/transition.html#//apple_ref/doc/uid/TP40001064-CH207-TPXREF101).

### C Primitives

**Note:** Objective-C inherits all of the C language primitive types and then adds a few extras.

#### Void

`void` is C's empty data type. It is most commonly used to specify the return type for functions that don't return anything.

#### Integers

Integers can be signed or unsigned.  When signed, they can be either positive or negative and when unsigned, they can only be positive.  Example: When declaring an `unsigned int`, the range of allowable integer values for a 32-bit compiler will shift from -2147483648 to +2147483647 to instead be 0 to +4294967295.

**Integer types with their accompanying byte sizes:**
```objC
// Char (1 byte for both 32-bit and 64-bit)
unsigned char anUnsignedChar = 255;
NSLog(@"char size: %zu", sizeof(char));

// Short (2 bytes for both 32-bit and 64-bit)
short aShort = -32768;
unsigned short anUnsignedShort = 65535;
NSLog(@"short size: %zu", sizeof(short));

// Integer (4 bytes for both 32-bit and 64-bit)
int anInt = -2147483648;
unsigned int anUnsignedInt = 4294967295;
NSLog(@"int size: %zu", sizeof(int));

// Long (4 bytes for 32-bit, 8 bytes for 64-bit)
long aLong = -9223372036854775808; // 32-bit
unsigned long anUnsignedLong = 18446744073709551615; // 32-bit
NSLog(@"long size: %zu", sizeof(long));

// Long Long (8 bytes for both 32-bit and 64-bit)
long long aLongLong = -9223372036854775808;
unsigned long long anUnsignedLongLong = 18446744073709551615;
NSLog(@"long long size: %zu", sizeof(long long));
```

**Fixed width integer types with their accompanying byte sizes as the variable names:**
```objC
// Exact integer types
int8_t aOneByteInt = 127;
uint8_t aOneByteUnsignedInt = 255;
int16_t aTwoByteInt = 32767;
uint16_t aTwoByteUnsignedInt = 65535;
int32_t aFourByteInt = 2147483647;
uint32_t aFourByteUnsignedInt = 4294967295;
int64_t anEightByteInt = 9223372036854775807;
uint64_t anEightByteUnsignedInt = 18446744073709551615;

// Minimum integer types
int_least8_t aTinyInt = 127;
uint_least8_t aTinyUnsignedInt = 255;
int_least16_t aMediumInt = 32767;
uint_least16_t aMediumUnsignedInt = 65535;
int_least32_t aNormalInt = 2147483647;
uint_least32_t aNormalUnsignedInt = 4294967295;
int_least64_t aBigInt = 9223372036854775807;
uint_least64_t aBigUnsignedInt = 18446744073709551615;

// The largest supported integer type
intmax_t theBiggestInt = 9223372036854775807;
uintmax_t theBiggestUnsignedInt = 18446744073709551615;
```

#### Floating Point

Floats cannot be signed or unsigned.

```objC
// Single precision floating-point (4 bytes for both 32-bit and 64-bit)
float aFloat = 72.0345f;
NSLog(@"float size: %zu", sizeof(float));

// Double precision floating-point (8 bytes for both 32-bit and 64-bit)
double aDouble = -72.0345f;
NSLog(@"double size: %zu", sizeof(double));

// Extended precision floating-point (16 bytes for both 32-bit and 64-bit)
long double aLongDouble = 72.0345e7L;
NSLog(@"long double size: %zu", sizeof(long double));
```

### Objective-C Primitives

**id** : Known as the anonymous or dynamic object type, it can store a reference to any type of **object** with no need to specify a pointer symbol.

```objC
id delegate = self.delegate;
```

**Class** : Used to denote an object's class and can be used for introspection of objects.

```objC
Class aClass = [UIView class];
```

**Method** : Used to denote a method and can be used for swizzling methods.

```objC
Method aMethod = class_getInstanceMethod(aClass, aSelector);
```

**SEL** : Used to specify a selector which is compiler-assigned code that identifies a method name.

```objC
SEL someSelector = @selector(someMethodName);
```

**IMP** : Used to point to the memory address of the start of a method.  You will probably never need to use this.

```objC
IMP theImplementation = [self methodForSelector:someSelector];
```

**BOOL** : Used to specify a boolean type where `0` is considered `NO` (false) and any non-zero value is considered `YES` (true).  Any `nil` object is also considered to be `NO` so there is no need to perform an equality check with `nil` (e.g. just write `if (someObject)` not `if (someObject != nil)`).

```objC
// Boolean
BOOL isBool = YES; // Or NO
```

**nil** : Used to specify a null object pointer.  When classes are first initialized, all properties of the class are set to `0` which means they point to `nil`.

Objective-C also has a number of other types such as `NSInteger`, `NSUInteger`, `CGRect`, `CGFloat`, `CGSize`, `CGPoint`, etc.

### Enum & Bitmask Types

Enumeration types can be defined a number of different ways:

```objC
// Specifying a typed enum with a name (recommended way)
typedef NS_ENUM(NSInteger, UITableViewCellStyle) {
    UITableViewCellStyleDefault,
    UITableViewCellStyleValue1,
    UITableViewCellStyleValue2,
    UITableViewCellStyleSubtitle
};

// Specify a bitmask with a name (recommended way)
typedef NS_OPTIONS(NSUInteger, RPBitMask) {
    RPOptionNone      = 0,
    RPOptionRight     = 1 << 0,
    RPOptionBottom    = 1 << 1,
    RPOptionLeft      = 1 << 2,
    RPOptionTop       = 1 << 3
};

// Other methods:

// Untyped
enum {
    UITableViewCellStyleDefault,
    UITableViewCellStyleValue1,
    UITableViewCellStyleValue2,
    UITableViewCellStyleSubtitle
};

// Untyped with a name
typedef enum {
    UITableViewCellStyleDefault,
    UITableViewCellStyleValue1,
    UITableViewCellStyleValue2,
    UITableViewCellStyleSubtitle
} UITableViewCellStyle;
```

#### Working with Bitmasks

```objC
// Set bits (only valid if it makes sense that your status may have many of the bitmask values)
RPBitMask status = RPOptionNone;
status |= RPOptionBottom;
status != RPOptionTop;

// Toggle bit
status ^= RPOptionTop;

// Set single bit to zero
status &= ! RPOptionBottom;

// Check if it matches a certain bit
if (status & RPOptionTop) { 
    [self doSometing]; 
}
```

### Casting to Data Types

Sometimes it is necessary to cast an `id` or different type into a specific class or data type.  Examples of this would be casting from a `float` to an `int` or from a `UITableViewCell` to a subclass such as `RPTableViewCell`.

Casting non-object data types:

```objC
// Format: nonObjectType variableName = (nonObjectType)variableNameToCastFrom;
int anInt = (int)anAnonymouslyTypedNonObjectOrDifferentDataType;
```

Casting object data types:

```objC
// Format: ClassNameOrObjectType *variableName = (ClassNameOrObjectType *)variableNameToCastFrom;
UIViewController *aViewController = (UIViewController *)anAnonymouslyTypedObjectOrDifferentDataType;
```

[Back to top](#objective-c-cheat-sheet)

## Constants

Using `const` is usually the better approach since `const` points to the same memory address for any references to that item in code. `#define` defines a macro which replaces all references with the actual constant value contents before compilation starts, instead of being a memory pointer to that constant value.

Constants can be defined as:

```objC
// Format: type const constantName = value;
NSString *const kRPShortDateFormat = @"MM/dd/yyyy";

// Format: #define constantName value
#define kRPShortDateFormat @"MM/dd/yyyy"
```

To make the constant available to external classes, you must also add it to the header `.h` file:

```objC
extern NSString *const kRPShortDateFormat;
```

If you know that a constant will only be available within it's containing implementation `.m` file, specify it as:

```objC
static NSString *const kRPShortDateFormat = @"MM/dd/yyyy";
```

A static variable declared within a method retains its value between invocations.  This can be useful when declaring a [singleton](#singletons) or creating custom setters/getters for a property.

[Back to top](#objective-c-cheat-sheet)

## Operators

#### Arithmetic Operators

Operator | Purpose
:---: | :---:
+ | Addition
- | Subtraction
* | Multiplication
/ | Division
% | Modulo

#### Relational and Equality Operators

Operator | Purpose
:---: | :---:
== | Equal to
!= | Not equal to
> | Greater than
< | Less than
>= | Greater than or equal to
<= | Less than or equal to

#### Logical Operators

Operator | Purpose
:---: | :---:
! | NOT
&& | Logical AND
&#124;&#124; | Logical OR

#### Compound Assignment Operators

Operator | Purpose
:---: | :---:
+= | Addition
-= | Subtraction
*= | Multiplication
/= | Division
%= | Modulo
&= | Bitwise AND
&#124;= | Bitwise Inclusive OR
^= | Exclusive OR
<<= | Shift Left
>>= | Shift Right

#### Increment and Decrement Operators

Operator | Purpose
:---: | :---:
++ | Addition
-- | Subtraction
*= | Multiplication
/= | Division
%= | Modulo
&= | Bitwise AND
&#124;= | Bitwise Inclusive OR
^= | Exclusive OR
<<= | Shift Left
>>= | Shift Right

#### Bitwise Operators

Operator | Purpose
:---: | :---:
& | Bitwise AND
&#124; | Bitwise Inclusive OR
^ | Exclusive OR
~ | Unary complement (bit inversion)
<< | Shift Left
>> | Shift Right

#### Other operators

Operator | Purpose
:---: | :---:
() | Cast
? : | Conditional
& | Memory Address
* | Pointer

[Back to top](#objective-c-cheat-sheet)

## Declaring Classes

Classes are declared using two files: a header (`.h`) file and an implementation (`.m`) file.

The header file should contain (in this order):
* Any needed `#import` statements or forward `@class` declarations
* Any protocol declarations
* An `@interface` declaration specifying which class you're inheriting from
* All publicly accessible variables, properties and methods

The implementation file should contain (in this order):
* Any needed `#import` statements
* An anonymous category, or class extension, for any private variables or properties
* An `@implementation` declaration specifying the class
* All public and private methods

Example:

MyClass.h

```objC
#import "SomeClass.h"

// Used instead of #import to forward declare a class in property return types, etc.
@class SomeOtherClass;

// Place all global constants at the top
extern NSString *const kRPErrorDomain;

// Format: YourClassName : ClassThatYouAreInheritingFrom
@interface MyClass : SomeClass

// Public properties first
@property (readonly, nonatomic, strong) SomeClass *someProperty;

// Then class methods
+ (id)someClassMethod;

// Then instance methods
- (SomeOtherClass *)doWork;

@end
```

MyClass.m

```objC
#import "MyClass.h"
#import "SomeOtherClass.h"

// Declare any constants at the top
NSString *const kRPErrorDomain = @"com.myIncredibleApp.errors";
static NSString *const kRPShortDateFormat = @"MM/dd/yyyy";

// Class extensions for private variables / properties
@interface MyClass ()
{
    int somePrivateInt;
}
// Re-declare as a private read-write version of the public read-only property
@property (readwrite, nonatomic, strong) SomeClass *someProperty;

@end

@implementation MyClass

// Use #pragma mark - statements to logically organize your code
#pragma mark - Class Methods

+ (id)someClassMethod
{
    return [[MyClass alloc] init];
}

#pragma mark - Init & Dealloc methods

- (id)init
{
    if (self = [super init]) {
        // Initialize any properties or setup code here
    }

    return self;
}

// Dealloc method should always follow init method
- (void)dealloc
{
    // Remove any observers or free any necessary cache, etc.

    [super dealloc];
}

#pragma mark - Instance Methods

- (SomeOtherClass *)doWork
{
    // Implement this
}

@end
```

#### Instantiation

When you want to create a new instance of a class, you use the syntax:

```objC
MyClass *myClass = [[MyClass alloc] init];
```

The `alloc` class method returns a pointer to a newly allocated block of memory large enough to store an instance of the class. The allocated memory contains zeros except for one instance variable, `isa`, that all Objective-C objects are required to have. The `isa` variable is automatically initialized to point to the class object that allocated the memory and enables the instance to receive messages such as `init` that are used to complete initialization.

[Back to top](#objective-c-cheat-sheet)

## Preprocessor Directives

This section needs a lot of love, so please feel free to improve upon it!

Directive | Purpose
:---: | ---
#define | Used to define constants or macros that are replaced by the compiler at runtime
#elif | An `else if` conditional statement
#else | An `else` conditional statement
#endif | An `end if` conditional statement
#error | Used to flag an error line in code
#if | An `if` conditional statement
#ifdef | An `if defined` conditional statement
#ifndef | An `if not defined` conditional statement
#import | Imports a header file. This directive is identical to `#include`, except that it won't include the same file more than once
#include | Includes a header file
#pragma | Used for commenting code or inhibiting compiler warnings
#undef | Used to undefine and redefine macros
#warning | Used to flag a warning line in code

####Special operator

The special operator `defined` is used in `#if` and `#elif` expressions to test whether a certain name is defined as a macro.

`defined` is useful when you wish to test more than one macro for existence at once. For example, `#if defined(__IPHONE_8_0) || defined(__MAC_10_9)`
would succeed if either of the names `__IPHONE_8_0` or `__MAC_10_9` is defined as a macro.

[Back to top](#objective-c-cheat-sheet)

## Compiler Directives

Also see the [literals](#literals) section.

#### Classes and Protocols

Directive | Purpose
:---: | ---
@class | Declares the names of classes defined elsewhere
@interface | Begins the declaration of a class or category interface
@implementation | Begins the definition of a class or category
@protocol | Begins the declaration of a formal protocol
@required | Declares the methods following the directive as required (default)
@optional | Declares the methods following the directive as optional. Classes implementing this protocol can decide whether to implement an optional method or not and should first check if the method is implemented before sending it a message.
@end | Ends the declaration/definition of a class, category, or protocol

#### Properties

Directive | Purpose
:---: | ---
@property | Declares a property with a backing instance variable
@synthesize | Synthesizes a property and allows the compiler to generate setters/getters for the backing instance variable
@dynamic | Tells the compiler that the setter/getter methods are not implemented by the class itself but somewhere else, like the superclass

#### Errors

Directive | Purpose
:---: | ---
@throw | Throws an exception
@try | Specifies a block of code to attempt
@catch | Specifies what to do if an exception was thrown in the `@try` block
@finally | Specifies code that runs whether an exception occurred or not

#### Visibility of Instance Variables

Directive | Purpose
:---: | ---
@private | Limits the scope of instance variables specified below it to the class that declares it
@protected | Limits the scope of instance variables specified below it to declaring and inheriting classes
@public | Removes restrictions on the scope of instance variables specified below it
@package | Declares the instance variables following the directive as public inside the framework that defined the class, but private outside the framework (64-bit only)

The default is `@protected`, so there is no need to explicitly specify this.

#### Others

Directive | Purpose
:---: | ---
@selector(method) | Returns the compiled selector that identifies a method
@protocol(name) | Returns the given protocol (an instance of the `Protocol` class)
@synchronized | Encapsulates code in a mutex lock to ensure that the block of code and the locked object can only be accessed by one thread at a time
@autoreleasepool | Replaces (and is 6 times faster than) the NSAutoreleasePool class
@encode(spec) | Yields a character string that encodes the type structure of `spec`
@compatibility_alias | Allows you to define an alias name for an existing class.
@defs(classname) | Yields the internal data structure of `classname` instances

[Back to top](#objective-c-cheat-sheet)

## Literals

Literals are compiler directives which provide a shorthand notation for creating common objects.

Syntax | What it does
:---: | ---
@"string" | Returns an `NSString` object
@28, @3.14, @YES | Returns an `NSNumber` object initialized with an appropriate class constructor, depending on the type
@[] | Returns an `NSArray` object
@{} | Returns an `NSDictionary` object
@() | Dynamically evaluates the boxed expression and returns the appropriate object literal based on its value

#### NSArray Access Syntax

```objC
NSArray *example = @[ @"hi", @"there", @23, @YES ];
NSLog(@"item at index 0: %@", example[0]);
```

#### NSDictionary Access Syntax

```objC
NSDictionary *example = @{ @"hi" : @"there", @"iOS" : @"people" };
NSLog(@"hi %@", example[@"hi"]);
```

#### Caveats

Similar to `NSString` literals, collection objects made via literal arrays and dictionaries are immutable. Instead, you will have to make a mutable copy after making the immutable dictionary or array.  Additionally, you cannot have static initializers like you can with `NSString`.

[Back to top](#objective-c-cheat-sheet)

## Methods

#### Declaration Syntax

For methods without a return type, use `void`:

```objC
// Does not return anything or take any arguments
- (void)someMethod;
```

`+` precedes declarations of class methods:

```objC
// Call on a class (e.g. [MyClass someClassMethod]);
+ (void)someClassMethod;
```

`-` precedes declarations of class instance methods:

```objC
// Called on an instance of a class (e.g. [[NSString alloc] init]);
- (void)someClassInstanceMethod;
```

Method arguments are declared after colons `:` and the method signature should describe the argument type:

```objC
// Does something with an NSObject argument
- (void)doWorkWithObject:(NSObject *)object;
```

Argument and return types are declared using type casting syntax:

```objC
// Returns an NSString object for the given NSObject arguments
- (NSString *)stringFromObject:(NSObject *)object andSomeOtherObject:(NSObject *)otherObject;
```

#### Calling Methods

Methods are called using bracket syntax: `[self someMethod];` or `[self sometMethodWithObject:object];`

`self` is a reference to the method's containing class.  The `self` variable is present in all Objective-C methods and it is one of two hidden arguments passed to code that implements a method, the other being `_cmd`, which identifies the received message.

At times, it is necessary to call a method in the superclass using `[super someMethod];`.

Under the hood, methods are implemented via message sending and they are turned into a variation of one of these two C functions:

```objC
id objc_msgSend(id self, SEL op, ...);
id objc_msgSendSuper(struct objc_super *super, SEL op, ...);
```

#### Testing Selectors

If you'd like to test if a class responds to a certain selector before you send it (and possibly crash), you can do so with:

```objC
if ([someClassOrInstance respondsToSelector:@selector(someMethodName)])
{
    // Call the selector or do something here
}
```

This pattern is common when you have a delegate implemented and need to test for methods declared as `@optional` before calling them on the delegate object.

[Back to top](#objective-c-cheat-sheet)

## Properties and Variables

Declaring a property allows you to maintain a reference to an object within a class or to pass objects between classes.

Public properties are declared in the header (`.h`) file:

```objC
@interface MyClass : NSObject

@property (readonly, nonatomic, strong) NSString *fullName;

@end
```

Private properties are declared in an anonymous category, or class extension, in the implementation (`.m`) file:

```objC
#import "MyClass.h"

// Class extension for private variables / properties
@interface MyClass ()
{
    // Instance variable
    int somePrivateInteger;
}
// Private properties
@property (nonatomic, strong) NSString *firstName;
@property (nonatomic, strong) NSString *lastName;
@property (readwrite, nonatomic, strong) NSString *fullName;

@end

@implementation MyClass

// Class implementation goes here

@end
```

The LLVM compiler automatically synthesizes all properties so there is no longer a need to explicitly write `@synthesize` statements for properties anymore.  When a property is synthesized, accessors are created which allow you to set and get the value of a property.

Even though you may not see them since they are created at build time, a getter/setter pair can be shown as:

```objC
- (BOOL)finished
{
    return _finished;
}
- (void)setFinished:(BOOL)aValue
{
    _finished = aValue;
}
```

You can overrride the getter and setter of a property to create customized behavior, or even use this pattern to create transient properties such as:

```objC
- (NSString *)fullName
{
    return [NSString stringWithFormat:@"%@ %@", self.firstName, self.lastName];
}
```

Properties are typically backed by an instance variable with a leading underscore, so creating a property called `firstName` would have a backing instance variable with the name `_firstName`.  You should only access that private instance variable if you override the getter/setter or if you need to setup the ivar in the class `init` method.

#### Property Attributes

When a property is specified, it is given the syntax:

```objC
@property SomeClass *someProperty;

// Or
@property (xxx) SomeClass *someProperty;
```

where `xxx` can be a combination of:

Type | What it does
:---: | ---
copy | Creates an immutable copy of the object upon assignment and is typically used for creating an immutable version of a mutable object.  Use this if you need the value of the object as it is at this moment, and you don't want that value to reflect any future changes made by other owners of the object.
assign |  Generates a setter which assigns the value directly to the instance variable, rather than copying or retaining it.  This is typically used for creating properties for primitive types (`float`, `int`, `BOOL`, etc).
weak | Variables that are `weak` can still point to objects but they do not become owners (or increase the retain count by 1). If the object's retain count drops to 0, the object will be deallocated from memory and the weak pointer will be set to `nil`.  It's best practice to create all delegates and `IBOutlet`'s as weak references since you do not own them.
unsafe_unretained | An unsafe reference is similar to a `weak` reference in that it doesn't keep its related object alive, but it won’t be set to `nil` if the object is deallocated.  This can lead to crashes due to accessing that deallocated object and therefore you should use `weak` unless the OS or class does not support it.
strong | This is the default and is required when the attribute is a pointer to an object. The automatically generated setter will retain (i.e. increment the retain count of) the object and keep the object alive until released.
readonly | This only generates a getter method so it won't allow the property to be changed via the setter method.
readwrite | This is the default and generates both a setter and a getter for the property.  Often times, a `readonly` property will be publicly defined and then a `readwrite` for the same property name will be privately redefined to allow mutation of the property value within that class only.
atomic | This is the default and means that any access operation is guaranteed to be uninterrupted by another thread and is typically slower in performance to use.
nonatomic | This is used to provide quicker (but thus interruptable) access operations.
getter=method | Used to specify a different name for the property's getter method.  This is typically done for boolean properties (e.g. `getter=isFinished`)
setter=method | Used to specify a different name for the property's setter method. (e.g. `setter=setProjectAsFinished`)

#### Accessing Properties

Properties can be accessed using either bracket syntax or dot notation, with dot notation being cleaner to read.

```objC
[self myProperty];

// Or
self.myProperty
```

#### Local Variables

Local variables exist only within the scope of a method.

```objC
- (void)doWork
{
   NSString *localStringVariable = @"Some local string variable.";
   [self doSomethingWithString:localStringVariable];
}
```

[Back to top](#objective-c-cheat-sheet)

## Naming Conventions

The general rule of thumb: Clarity and brevity are both important, but clarity should never be sacrificed for brevity.

#### Methods and Properties

These both use `camelCase` where the first letter of the first word is lowercase and the first letter of each additional word is capitalized.

#### Class names and Protocols

These both use `CapitalCase` where the first letter of every word is capitalized.

#### Methods

These should use verbs if they perform some action (e.g. `performInBackground`).  You should be able to infer what is happening, what arguments a method takes, or what is being returned just by reading a method signature.

Example:

```objC
// Correct
- (UITableViewCell *)tableView:(UITableView *)tableView cellForRowAtIndexPath:(NSIndexPath *)indexPath
{
    // Code
}

// Incorrect (not expressive enough)
- (UITableViewCell *)table:(UITableView *)tableView cell:(NSIndexPath *)indexPath
{
    // Code
}
```

#### Properties and Local Variables

When using properties, instance variables are internally created with a preceeding underscore, so `myVariableName` is created as `_myVariableName`.  However, Objective-C now synthesizes these properties for you so you should never have to access that underscored instance variable directly except in a custom setter.

Instead, instance variables should always be accessed and mutated using `self.`

Local variables should not contain underscores.

#### Constants

These should start with `k` and `XXX` where `XXX` is a prefix, possibly your initials, to avoid naming conflicts.  You should not be afraid to be expressive with your constant naming, especially if it's a global constant.  Using `kRPNavigationFadeOutAnimationDuration` is much better than `fadeOutTiming`.

[Back to top](#objective-c-cheat-sheet)

## Blocks

Blocks are essentially anonymous functions that are used to pass arbitrary code between methods or to execute code as a callback within a method.  Since blocks are implemented as closures, the surrounding state is also captured (which can sometimes lead to retain cycles).

#### Syntax

```objC
// As a local variable
returnType (^blockName)(parameterTypes) = ^returnType(parameters) {
    // Block code here
};

// As a property
@property (nonatomic, copy) returnType (^blockName)(parameterTypes);

// As a method parameter
- (void)someMethodThatTakesABlock:(returnType (^)(parameterTypes))blockName {
    // Block code here
};

// As an argument to a method call
[someObject someMethodThatTakesABlock: ^returnType (parameters) {
    // Block code here
}];

// As a typedef
typedef returnType (^TypeName)(parameterTypes);
TypeName blockName = ^(parameters) {
    // Block code here
};
```

#### Mutating block variables

Since variables within a block are just snapshots of what they were outside of the block scope, you must preceed any variables that you want to mutate within the block with `__block`, such as:

```objC
__block int someIncrementer = 0;
[someObject someMethodThatTakesABlock:^{
    someIncrementer++;
}];
```

#### Retain cycles

Since blocks strongly capture all variables within the scope of the block, you have to be careful how you setup your blocks code.  Here are two examples of retain cycles:

```objC
[someObject someMethodThatTakesABlock:^{
    [someObject performSomeAction];  // Will raise a warning
}];

[self someMethodThatTakesABlock:^{
    [self performSomeAction];       // Will raise a warning
}];
```

In both of these cases, the object which performs the block owns the block, which also owns the object. This creates a loop, or a retain cycle, which means the memory is eventually leaked.

To get around this warning you can either refactor the code to be:

```objC
[self someMethodThatTakesABlock:^{
    [object performSomeAction];   // No retain cycle here
}];
```

Or you can use a `__weak` object:

```objC
__weak typeof(self) weakSelf = self;
[self someMethodThatTakesABlock:^{
    [weakSelf performSomeAction];  // No retain cycle here
}];
```

[Back to top](#objective-c-cheat-sheet)

## Control Statements

Objective-C uses all of the same control statements that other languages have:

#### If-Else If-Else

```objC
if (someTestCondition) {
    // Code to execute if the condition is true
} else if (someOtherTestCondition) {
    // Code to execute if the other test condition is true
} else {
    // Code to execute if the prior conditions are false
}
```

#### Ternary Operators

The shorthand notation for an `if-else` statement is a ternary operator of the form: `someTestCondition ? doIfTrue : doIfFalse;`

Example:

```objC
- (NSString *)stringForTrueOrFalse:(BOOL)trueOrFalse
{
    return trueOrFalse ? @"True" : @"False";
}
```

There is also another lesser known form: `A ?: B;` which basically returns `A` if `A` is `YES` or non-nil, otherwise it returns `B`.

#### For Loops

```objC
for (int i = 0; i < totalCount; i++) {
    // Code to execute while i < totalCount
}
```

#### Fast Enumeration

```objC
for (Person *person in arrayOfPeople) {
    // Code to execute each time
}
```

where `arrayOfPeople` can be any object that conforms to the `NSFastEnumeration` protocol.  `NSArray` and `NSSet` enumerate over their objects, `NSDictionary` enumerates over keys, and `NSManagedObjectModel` enumerates over entities.

#### While Loop

```objC
while (someTextCondition) {
   // Code to execute while the condition is true
}
```

#### Do While Loop

```objC
do {
    // Code to execute while the condition is true
} while (someTestCondition);
```

#### Switch

Switch statements are often used in place of `if` statements if there is a need to test if a certain variable matches the value of another constant or variable.  For example, you may want to test if an error code integer you received matches an existing constant value or if it's a new error code.

```objC
switch (errorStatusCode)
{
    case kRPNetworkErrorCode:
        // Code to execute if it matches
        break;

     case kRPWifiErrorCode:
        // Code to execute if it matches
        break;

     default:
        // Code to execute if nothing else matched
        break;
}
```

#### Exiting Loops

* `return` : Stops execution and returns to the calling function.  It can also be used to return a value from a method.
* `break` : Used to stop execution of a loop.

Newer enumeration methods now have special `BOOL` variables (e.g. `BOOL *stop`) that are used to stop loop execution.  Setting that variable to `YES` within the loop is similar to calling `break`.

[Back to top](#objective-c-cheat-sheet)

## Enumeration

Fast enumeration was already mentioned in the [control statements](#control-statements) section, but many collection classes also have their own block-based methods for enumerating over a collection.

Block-based methods perform almost as well as using fast enumeration, but they just provide extra options for enumerating over collections.  An example of block-based enumeration over an `NSArray` would be:

```objC
NSArray *people = @[ @"Bob", @"Joe", @"Penelope", @"Jane" ];
[people enumerateObjectsUsingBlock:^(NSString *nameOfPerson, NSUInteger idx, BOOL *stop) {
    NSLog(@"Person's name is: %@", nameOfPerson);
}];
```

[Back to top](#objective-c-cheat-sheet)

## Extending Classes

There are a few different ways to extend a class in Objective-C, with some approaches being much easier than others.

Approach | Difficulty | Purpose
:---: | :---: | ---
[Inheritance](#inheritance) | Easy | Used if all you want to do is inherit behavior from another class, such as `NSObject`
[Category](#categories) | Easy | Used if all you need to do is add additional methods to that class.  If you also need to add instance variables to an existing class using a category, you can fake this by using associative references.
[Delegation](#delegation) | Easy | Used to allow one class to react to changes in or influence behavior of another class while minimizing coupling.
[Subclass](#subclassing) | Can be difficult | Used if you need to add methods and properties to an existing class or if you want to inherit behavior from an existing class.  Some classes are not designed to be subclassed.
[Swizzle](#swizzling) | Can be difficult | Swizzling allows you to replace a method in an existing class with one of your own making.  This approach can lead to a lot of unexpected behavior, so it should be used very sparingly.

### Inheritance

Inheritance essentially allows you to create concrete [subclasses](#subclassing), which typically have specialized behavior, while inheriting all methods and properties from a superclass which are specified as `@public` or `@protected` within the superclass' header file.

Looking through any framework or open source project, you can see the use of inheritance to not only get behavior for free, but to also consolidate code and allow it to be easily reused.  Examples of this approach can be seen with any of the mutable framework classes such as `NSMutableString` which is a subclass of `NSString`.

In Objective-C, all objects have much of their behavior defined by the `NSObject` class through the act of class inheritance.  Without inheritance, you would have to implement common methods like object or class equality checks on your own and you'd end up with a lot of duplicate code across classes.

```objC
// MyClass inherits all behavior from the NSObject class
@interface MyClass : NSObject

// Class implementation

@end
```

Inheritance inherently creates coupling between classes, so be sure to take this into consideration.

### Categories

Categories are a very useful and easy way of extending classes, especially when you don't have access to the original source code (such as for Cocoa Touch frameworks and classes). A category can be declared for any class and any methods that you declare in a category will be available to all instances of the original class, **as well as any subclasses of the original class**. At runtime, there's no difference between a method added by a category and one that is implemented by the original class.

Categories are also useful to:

* Declare informal protocols
* Group related methods similar to having multiple classes
* Break up a large class implementation into multiple categories, which helps with incremental compilation
* Easily configure a class differently for different applications

#### Implementation

Categories are named with the format: `ClassYouAreExtending` + `DescriptorForWhatYouAreAdding`

As an example, let's say that we need to add a new method to the `UIImage` class (and all subclasses) that allows us to easily resize or crop instances of that class.  You'd then need to create a header/implementation file pair named `UIImage+ResizeCrop` with the following implementation:

UIImage+ResizeCrop.h

```objC
@interface UIImage (ResizeCrop)

- (UIImage *)croppedImageWithSize:(CGSize)size;
- (UIImage *)resizedImageWithSize:(CGSize)size;

@end
```

UIImage+ResizeCrop.m

```objC
#import "UIImage+ResizeCrop.h"

@implementation UIImage (ResizeCrop)

- (UIImage *)croppedImageWithSize:(CGSize)size
{
    // Implementation code here
}

- (UIImage *)resizedImageWithSize:(CGSize)size
{
    // Implementation code here
}
```

You could then call these methods on any instances of `UIImage` or it's subclasses such as:

```objC
UIImage *croppedImage = [userPhoto croppedImageWithSize:photoSize];
```

#### Associative References

Unless you have access to the source code for a class at compile time, it is not possible to add instance variables and properties to a class by using a category.  Instead, you have to essentially fake this by using a feature of the Objective-C runtime called **associative references**.

For example, let's say that we want to add a public property to the `UIScrollView` class to store a reference to a `UIView` object, but we don't have access to `UIScrollView`'s source code.  We would have to create a category on `UIScrollView` and then create a pair of getter/setter methods for this new property to store a reference like this:

UIScrollView+UIViewAdditions.h

```objC
#import <UIKit/UIKit.h>

@interface UIScrollView (UIViewAdditions)

@property (nonatomic, strong) UIView *myCustomView;

@end
```

UIScrollView+UIViewAdditions.m

```objC
#import "UIScrollView+UIViewAdditions.h"
#import <objc/runtime.h>

static char UIScrollViewMyCustomView;

@implementation UIScrollView (UIViewAdditions)

@dynamic myCustomView;

- (void)setMyCustomView:(UIView *)customView
{
    [self willChangeValueForKey:@"myCustomView"];
    objc_setAssociatedObject(self, &UIScrollViewMyCustomView, customView, OBJC_ASSOCIATION_RETAIN_NONATOMIC);
    [self didChangeValueForKey:@"myCustomView"];
}

- (UIView *)myCustomView {
    return objc_getAssociatedObject(self, &UIScrollViewMyCustomView);
}

@end
```

Let's explain a bit about what's happening here:

* We create a static key called `UIScrollViewMyCustomView` that we can use to get and set the associated object.  Declaring it as `static` ensures that it is unique since it always points to the same memory address.
* Next, we declare the property we are adding as `@dynamic` which tells the compiler that the getter/setter is not implemented by the `UIScrollView` class itself.
* Within the setter, we use `willChangeValueForKey` followed by `didChangeValueForKey` to ensure that we notify any key-value observers of a change to this property.
* Within the setter, we use `objc_setAssociatedObject` to store a reference to the object that we really care about, `customView` under the static key that we created. `&` is used to denote that it is a pointer to a pointer to `UIScrollViewMyCustomView`
* Within the getter, we retrieve the object reference using `objc_getAssociatedObject` and a pointer to the static key

We could then use the property just like we would any other property:

```objC
UIScrollView *scrollView = [[UIScrollView alloc] init];
scrollView.myCustomView = [[UIView alloc] init];
NSLog(@"Custom view is %@", scrollView.myCustomView);
// Custom view is <UIView: 0x8e4dfb0; frame = (0 0; 0 0); layer = <CALayer: 0x8e4e010>>
```

More info from the docs:

>The [objc_setAssociatedObject] function takes four parameters: the source object, a key, the value, and an association policy constant. The key is a void pointer.
>
>The key for each association must be unique. A typical pattern is to use a static variable. The policy specifies whether the associated object is assigned, retained, or copied, and whether the association is be made atomically or non-atomically. This pattern is similar to that of the attributes of a declared property

The possible property declaration attributes are:

>OBJC_ASSOCIATION_RETAIN_NONATOMIC, OBJC_ASSOCIATION_ASSIGN, OBJC_ASSOCIATION_COPY_NONATOMIC, OBJC_ASSOCIATION_RETAIN, OBJC_ASSOCIATION_COPY

#### Class Extension Categories

In the section about [declaring classes](#declaring-classes), it shows how the private instance variables and properties within a class are actually added to the class by using an anonymous (unnamed) category, also known as a class extension.

```objC
// Class extensions for private variables / properties
@interface MyClass ()
{
    int somePrivateInt;

    // Re-declare as a private read-write version of the public read-only property
    @property (readwrite, nonatomic, strong) SomeClass *someProperty;
}
@end
```

Class extensions are the only way that you can add variables and properties using a category, so you must have access to the source code at compile time in order to use this method.

#### Core Data Categories

Categories are very useful when you are working with Core Data models and want to add additional methods to an `NSManagedObject` subclass without worrying about Xcode writing over the model class each time you migrate a model.

Model classes should be kept to a minimum, containing only the model properties and Core Data generated accessor methods.  All others, such as transient methods, should be implemented in a category on the model class.

#### Naming Conflicts

Great advice from [Apple's docs](https://developer.apple.com/library/ios/documentation/cocoa/conceptual/ProgrammingWithObjectiveC/CustomizingExistingClasses/CustomizingExistingClasses.html):

>Because the methods declared in a category are added to an existing class, you need to be very careful about method names.
>
>If the name of a method declared in a category is the same as a method in the original class, or a method in another category on the same class (or even a superclass), the behavior is undefined as to which method implementation is used at runtime. This is less likely to be an issue if you’re using categories with your own classes, but can cause problems when using categories to add methods to standard Cocoa or Cocoa Touch classes.

### Delegation

Delegation is basically a way to allow one class to react to changes made in another class or to influence the behavior of another class while minimizing coupling between the two.

The most commonly known example of the delegate pattern in iOS is with `UITableViewDelegate` and `UITableViewDataSource`.  When you tell the compiler that your class conforms to these protocols, you are essentially agreeing to implement certain methods within your class that are required for a `UITableView` to properly function.

#### Conforming to an Existing Protocol

To conform to an existing protocol, import the header file that contains the protocol declaration (not necessary for framework classes).  Then, insert the protocol name within `< >` symbols and separate multiple protocols by a comma.  Both options shown below will work just fine, but I prefer to keep them in the header file since it feels cleaner to me.

**Option 1**: In your `.h` file:

```objC
#import "RPLocationManager.h"

@interface MyViewController : UIViewController <RPLocationManagerDelegate, UITableViewDelegate, UITableViewDataSource>

@end
```

**Option 2**: In the `.m` file:

```objC
#import "MyViewController.h"

@interface MyViewController () <RPLocationManagerDelegate, UITableViewDelegate, UITableViewDataSource>
{
    // Class extension implementation
}
@end
```

#### Creating Your Own Protocol

To create your own protocol for other classes to conform to, follow this syntax:

RPLocationManager.h

```objC
#import <SomeFrameworksYouNeed.h>

// Declare your protocol and decide which methods are required/optional
// for the delegate class to implement
@protocol RPLocationManagerDelegate <NSObject>
- (void)didAcquireLocation:(CLLocation *)location;
- (void)didFailToAcquireLocationWithError:(NSError *)error;
@optional
- (void)didFindLocationName:(NSString *)locationName;
@end

@interface RPLocationManager : NSObject

// Create a weak, anonymous object to store a reference to the delegate class
@property (nonatomic, weak) id <RPLocationManagerDelegate>delegate;

// Implement any other methods here

@end
```

When we declare the `@protocol` named `RPLocationManagerDelegate`, all methods are defaulted to being `@required` so it's not necessary to explicitly state this.  However, if you want certain methods to be `@optional` for conforming classes to implement, you must state this.

Additionally, it is necessary to weakly declare an anonymously typed property called `delegate` which also references the `RPLocationManagerDelegate` protocol.

#### Sending Delegate Messages

In the example above, `RPLocationManager.h` declares some methods that the class which is acting as the delegate must implement.  Within `RPLocationManager.m` itself, you could implement these a few different ways, but we'll just show two cases: a) required methods; b) optional methods.

**Required Methods**

```objC
- (void)updateLocation
{
    // Perform some work

    // When ready, notify the delegate method
    [self.delegate didAcquireLocation:locationObject];
}
```

**Optional Methods**

```objC
- (void)reverseGeoCode
{
    // Perform some work

    // When ready, notify the delegate method
    if ([self.delegate respondsToSelector:@selector(didFindLocationName:)]) {
        [self.delegate didFindLocationName:locationName];
    }
}
```

The only difference between `@required` and `@optional` methods is that you should always check if the referenced `delegate` implemented an optional method before calling it on that class.

#### Implementing Delegate Methods

To implement a delegate method, just conform to the protocol like was discussed earlier, and then write it as if it was a normal method:

MyViewController.m

```objC
- (void)didFindLocationName:(NSString *)locationName
{
    NSLog(@"We found a location with the name %@", locationName);
}
```

### Subclassing

Subclassing is essentially the same as [inheritance](#inheritance), but you would typically create the subclass to either

* Override a method or property implementation in the superclass; or
* Create specialized behavior for the subclass (e.g. `Toyota` is a subclass of `Car`... it still has tires, an engine, etc, but it has additional custom behavior that uniquely makes it a `Toyota`)

Many design patterns, such as [categories](#categories) and [delegation](#delegation), exist so that you don't have to subclass another class.  For example, the `UITableViewDelegate` protocol was created to allow you to provide the implementation of methods like `tableView:didSelectRowAtIndexPath:` in your own class instead of having to subclass `UITableView` to override that method.

Other times, classes like `NSManagedObject` are designed to be easily subclassed.  The general rule of thumb is to subclass another class only if you can satisfy the [Liskov substitution principle](http://en.wikipedia.org/wiki/Liskov_substitution_principle):

>If S is a subtype of T, then objects of type T in a program may be replaced with objects of type S without altering any of the desirable properties of that program.

#### Example

Let's assume that we want to model cars.  All cars have similar behavior and characteristics, so let's put some of that in a superclass called `Car`.

Car.h

```objC
#import <Foundation/Foundation.h>

@interface Car : NSObject

@property (nonatomic, strong) NSString *make;
@property (nonatomic, strong) NSString *model;
@property (nonatomic, assign) NSInteger year;

- (void)startEngine;
- (void)pressGasPedal;
- (void)pressBrakePedal;

@end
```

Car.m

```objC
#import "Car.h"

@implementation Car

- (void)startEngine
{
   NSLog(@"Starting the engine.");
}

- (void)pressGasPedal
{
    NSLog(@"Accelerating...");
}

- (void)pressBrakePedal
{
    NSLog(@"Decelerating...");
}

@end
```

Now when we want to spawn off new car makes and models with unique characteristics, we use the `Car` superclass as a starting point and then add custom behavior in the subclass.

Toyota.h

```objC
#import "Car.h"

@interface Toyota : Car

- (void)preventAccident;

@end
```

Toyota.m

```objC
#import "Toyota.h"

@implementation Toyota

- (void)startEngine
{
    // Perform custom start sequence, different from the superclass

    NSLog(@"Starting the engine.");
}

- (void)preventAccident
{
    [self pressBrakePedal];

    [self deployAirbags];
}

- (void)deployAirbags
{
   NSLog(@"Deploying the airbags.");
}

@end
```

Even though `pressBrakePedal` is declared in the `Car` class, that method is still accessible in the `Toyota` class due to inheritance.

#### Designated Initializers

Often times, classes implement designated class initializers to allow for easy instantiation.  If you override the main designated initializer for a class or provide a new designated initializer, it's important to ensure that you also override all other designated initializers so they use your new implementation instead of the superclass version.  If you forget to do so and someone calls one of the secondary designated initializers on your subclass, they will get behavior from the superclass instead.

```objC
// The new designated initializer for this class
- (instancetype)initWithFullName:(NSString *)fullName
{
    if (self = [super init]) {
        _fullName = fullName;
        [self commonSetup];
    }
    return self;
}

// Provide a sensible default for other initializers
- (instancetype)init
{
    return [self initWithFullName:@"Default User"];
}
```

If you'd rather someone not use a default initializer for some rare case, you should throw an exception and provide them with an alternative solution:

```objC
- (instancetype)init {
        [NSException raise:NSInvalidArgumentException
                format:@"%s Using the %@ initializer directly is not supported. Use %@ instead.", __PRETTY_FUNCTION__, NSStringFromSelector(@selector(init)), NSStringFromSelector(@selector(initWithFrame:))];
    return nil;
}
```

#### Overriding Methods

If you're subclassing another class to override a method within that class, you must be a little cautious.  If you want to maintain the same behavior as the superclass, but just modify it slightly, you can call `super` within the override like this:

```objC
- (void)myMethod
{
    [super myMethod];

    // Provide your additional custom behavior here
}
```

If you don't want any of the superclass's behavior for the overridden method, simply leave out that call to `super`, but be careful that there aren't any memory or object lifecycle consequences for doing so.

Additionally, if the superclass has primitive methods upon which other derived methods are implemented, you must ensure that you override all necessary primitive methods necessary for the derived methods to work properly.

#### Caveats

Certain classes don't lend themselves well to being easily subclassed and therefore subclassing is discouraged in those cases.  An example of this is when trying to subclass a class cluster such as `NSString` or `NSNumber`.  Class clusters have quite a few private classes within them so it's difficult to ensure that you have overridden all of the primitive methods and designated initializers within the class cluster properly.

### Swizzling

As is often the case, clarity is better than cleverness.  As a general rule, it's typically better to work around a bug in a method implementation than it is to replace the method by using method swizzling.  The reason being that other people using your code might not realize that you replaced the method implementation and then they are stuck wondering why a method isn't responding with the default behavior.

For this reason, we don't discuss swizzling here but you are welcome to [read up on it here](http://www.mikeash.com/pyblog/friday-qa-2010-01-29-method-replacement-for-fun-and-profit.html).

[Back to top](#objective-c-cheat-sheet)

## Error Handling

Errors are typically handled three different ways: assertions, exceptions, and recoverable errors.  In the case of assertions and exceptions, they should only be used in rare cases since crashing your app is obviously not a great user experience.

### Assertions

Assertions are used when you want to ensure that a value is what it is supposed to be.  If it's not the correct value, you force exit the app.

```objC
NSAssert(someCondition, @"The condition was false, so we are exiting the app.");
```

>`Important:` Do not call functions with side effects in the condition parameter of this macro. The condition parameter is not evaluated when assertions are disabled, so if you call functions with side effects, those functions may never get called when you build the project in a non-debug configuration.

### Exceptions

Exceptions are only used for programming or unexpected runtime errors.  Examples: attempting to access the 6th element of an array with 5 elements (out-of-bounds access), attempting to mutate immutable objects, or sending an invalid message to an object. You usually take care of these sorts of errors with exceptions when an application is being created rather than at runtime.

An example of this might be if you have a library which requires an API key to use.

```objC
// Check for an empty API key
- (void)checkForAPIKey
{
    if (!self.apiKey || !self.apiKey.length) {
        [NSException raise:@"Forecastr" format:@"Your Forecast.io API key must be populated before you can access the API.", nil];
    }
}
```

#### Try-Catch

If you're worried that a block of code is going to throw an exception, you can wrap it in a try-catch block but keep in mind that this has slight performance implications

```objC
@try {
    // The code to try here
}
@catch (NSException *exception) {
    // Handle the caught exception
}
@finally {
    // Execute code here that would run after both the @try and @catch blocks
}
```

### Recoverable Errors

Many times, methods will return an `NSError` object in a failure block or as a pointer to a pointer (in the case of `NSFileManager`).  These are typically returned for recoverable errors and offer a much more pleasant user experience since they can clue the user into what just went wrong.

```objC
[forecastr getForecastForLocation:location success:^(id JSON) {
    NSLog(@"JSON response was: %@", JSON);
} failure:^(NSError *error, id response) {
    NSLog(@"Error while retrieving forecast: %@", error.localizedDescription);
}];
```

#### Creating Your Own Errors

It's also possible to create your own `NSError` objects to return in methods.

```objC
// Error domain & enums
NSString *const kFCErrorDomain = @"com.forecastr.errors";
typedef NS_ENUM(NSInteger, ForecastErrorType) {
    kFCCachedItemNotFound,
    kFCCacheNotEnabled
};

@implementation Forecastr

- (void)checkForecastCacheForURLString:(NSString *)urlString
                               success:(void (^)(id cachedForecast))success
                               failure:(void (^)(NSError *error))failure
{
    // Check cache for a forecast
    id cachedItem = [forecastCache objectForKey:urlString];
    if (cachedItem) {
        success(cachedItem);
    } else {
        // Return an error since it wasn't found
        failure([NSError errorWithDomain:kFCErrorDomain code:kFCCachedItemNotFound userInfo:nil]);
    }
}

@end
```

[Back to top](#objective-c-cheat-sheet)

## Passing Information

We have already discussed many ways of passing information between classes, such as through methods or delegates, but we'll discuss a few more here and also give one more example of [delegation](#delegation).

### Passing through Delegate

A very common way to pass data from one view controller to another is to use a delegate method.  An example of this would be if you had a modal view with a table that showed over top of your view controller and you needed to know which table cell the user pressed.

AddPersonViewController.h (the modal view)

```objC
#import <UIKit/UIKit.h>
#import "Person.h"

@protocol AddPersonTableViewControllerDelegate <NSObject>
- (void)didSelectPerson:(Person *)person;
@end

@interface AddPersonTableViewController : UITableViewController

@property (nonatomic, weak) id <AddPersonTableViewControllerDelegate>delegate;

@end
```

AddPersonViewController.m

```objC
// Other implementation details left out

- (void)tableView:(UITableView *)tableView didSelectRowAtIndexPath:(NSIndexPath *)indexPath
{
    Person *person = [people objectAtIndex:indexPath.row];
    [self.delegate didSelectPerson:person];
}
```

GroupViewController.m (the normal view)

```objC
// Other implementation details left out, such as showing the modal view
// and setting the delegate to self

#pragma mark - AddPersonTableViewControllerDelegate

- (void)didSelectPerson:(Person *)person
{
    [self dismissViewControllerAnimated:YES completion:nil];

    NSLog(@"Selected person: %@", person.fullName);
}
```

We left out a few implementation details, such as conforming to the `AddPersonTableViewControllerDelegate`, but you are welcome to read the [delegation](#delegation) section for those.

Also, notice that we dismiss the modal view controller (`AddPersonViewController`) in the same class that originally showed it.  This is the recommended approach by Apple.

### NSNotificationCenter

Notifications are broadcast messages that are used to decouple classes and establish anonymous communication between objects at runtime.  Notifications may be posted by any number of objects and received by any number of objects thus enabling one-to-many and many-to-many relationships between objects.

**Note**: Notifications are sent synchronously so if your observer method takes a long time to return, you are essentially prevently the notification from being sent to other observing objects.

#### Registering Observers

You can register to be notified when a certain event has happened, including system notifications, such as a `UITextField` which has begun editing.

```objC
[[NSNotificationCenter defaultCenter] addObserver:self selector:@selector(textFieldDidBeginEditing:)
                                             name:UITextFieldTextDidBeginEditingNotification object:self];
```

When the `UITextFieldTextDidBeginEditingNotification` notification is broadcast by the OS framework, the `textFieldDidBeginEditing:` will be called by `NSNotificationCenter` and an object will be sent along with the notification that could contain data.

A possible implementation of the `textFieldDidBeginEditing:` method could be:

```objC
#pragma mark - Text Field Observers

- (void)textFieldDidBeginEditing:(NSNotification *)notification
{
    // Optional check to make sure the method was called from the notification
    if ([notification.name isEqualToString:UITextFieldTextDidBeginEditingNotification])
    {
        // Do something
    }
}
```

#### Removing Observers

It's important to remove yourself as an observer when the class is deallocated, otherwise `NSNotificationCenter` will attempt to call a method on a deallocated class and a crash will ensue.

```objC
- (void)dealloc
{
    [[NSNotificationCenter defaultCenter] removeObserver:self];
}
```

#### Posting Notifications

You can also create and post your own notifications.  It's best practice to keep notification names in a [constants](#constants) file so that you don't accidentally misspell one of the notification names and sit there trying to figure out why the notification wasn't sent/received.

Naming notifications:

Notifications are identified by `NSString` objects whose names are composed in this way:

>[Name of associated class] + [Did | Will] + [UniquePartOfName] + Notification

Declare a string constant, using the notification name as the string's value:

```objC
// Remember to put the extern of this in the header file
NSString *const kRPAppDidResumeFromBackgroundNotification = @"RPAppDidResumeFromBackgroundNotification";
```

Post notification:

```objC
[[NSNotificationCenter defaultCenter] postNotificationName:kRPAppDidResumeFromBackgroundNotification object:self];
```

### View Controller Properties

When you are preparing to display a new view controller, you can assign data to one of it's properties prior to display:

```objC
MyViewController *myVC = [[MyViewController alloc] init];
myVC.someProperty = self.someProperty;
[self presentViewController:myVC animated:YES completion:nil];
```

### Storyboard Segue

When you are transitioning from one view controller to another in a storyboard, there is an easy way to pass data between the two by implementing the `prepareForSegue:sender:` method:

```objC
#pragma mark - Segue Handler

- (void)prepareForSegue:(UIStoryboardSegue *)segue sender:(id)sender
{
    if ([segue.identifier isEqualToString:@"showLocationSearch"] {
        [segue.destinationViewController setDelegate:self];
    } else if ([segue.identifier isEqualToString:@"showDetailView"]) {
        DetailViewController *detailView = segue.destinationViewController;
        detailView.location = self.location;
    }
}
```

## User Defaults

User defaults are basically a way of storing simple preference values which can be saved and restored across app launches.  It is not meant to be used as a data storage layer, like Core Data or sqlite.

### Storing Values

```objC
NSUserDefaults *userDefaults = [NSUserDefaults standardUserDefaults];
[userDefaults setValue:@"Some value" forKey:@"RPSomeUserPreference"];
[userDefaults synchronize];
```

Always remember to call `synchronize` on the defaults instance to ensure they are saved properly.

### Retrieving Values

```objC
NSUserDefaults *userDefaults = [NSUserDefaults standardUserDefaults];
id someValue = [userDefaults valueForKey:@"RPSomeUserPreference"];
```

There are also other convenience methods on `NSUserDefaults` instances such as `boolForKey:`, `stringForKey:`, etc.

[Back to top](#objective-c-cheat-sheet)

## Common Patterns

### Singletons

Singleton's are a special kind of class where only one instance of the class exists for the current process. They are a convenient way to share data between different parts of an app without creating global variables or having to pass the data around manually, but they should be used sparingly since they often create tighter coupling between classes.

To turn a class into a singleton, you place the following method into the implementation (`.m`) file, where the method name is prefixed with `shared` plus another word which best describes your class.  For example, if the class is a network or location manager, you would name the method `sharedManager` instead of `sharedInstance`.

```objC
+ (instancetype)sharedInstance
{
   static id sharedInstance = nil;
   static dispatch_once_t onceToken;
   dispatch_once(&onceToken, ^{
      sharedInstance = [[self alloc] init];
   });

   return sharedInstance;
}
```
The use of `dispatch_once` ensures that this method is only ever executed once, even if it's called multiple times across many classes or different threads.

If the above code were placed within `MyClass`, then you would get a reference to that singleton class in another class with the following code:

```objC
MyClass *myClass = [MyClass sharedInstance];
[myClass doSomething];
NSLog(@"Property value is %@", myClass.someProperty);
```

[Back to top](#objective-c-cheat-sheet)
