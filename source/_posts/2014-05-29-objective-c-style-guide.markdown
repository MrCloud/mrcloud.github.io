---
layout: post
title: "Objective-C Style Guide"
date: 2014-05-29 22:00:37 +0200
comments: true
categories: Objective-C
---

Hi, this post sums up the coding and naming conventions I'm using to develop for
Apple platforms.

---
## Table of Contents

* [Language](#language)
* [Operators](#operators)
* [Types](#types)
* [Code Organization](#code-organization)
* [Spacing](#spacing)
* [Comments](#comments)
* [Naming](#naming)
  * [Underscores](#underscores)
* [Methods](#methods)
* [Variables](#variables)
* [Property Attributes](#property-attributes)
* [Dot-Notation Syntax](#dot-notation-syntax)
* [Literals](#literals)
* [Constants](#constants)
* [Enumerated Types](#enumerated-types)
* [Bitmasks](#bitmasks)
* [Case Statements](#case-statements)
* [Private Properties](#private-properties)
* [Image Naming](#image-naming)
* [Booleans](#booleans)
* [Conditionals](#conditionals)
  * [Ternary Operator](#ternary-operator)
* [Init Methods](#init-methods)
* [Class Constructor Methods](#class-constructor-methods)
* [Inheritance & Super](#inheritance-super)
* [CGRect Functions](#cgrect-functions)
* [Golden Path](#golden-path)
* [Exceptions & Error handling](#error-handling)
* [Singletons](#singletons)
* [Line Breaks](#line-breaks)
* [Imports](#imports)
* [Xcode Project](#xcode-project)

<!--more-->

## <a name="language"></a>Language

Even if I'm French, I always use US English for code & comments in order to
maintain coherence with Cocoa and third party Framework naming.

**Preferred:**
```objc
UIColor *myColor = [UIColor whiteColor];
```


**Not Preferred:**
```objc
UIColor *myColour = [UIColor whiteColor];
```

## <a name="operators"></a>Operators

```objc
NSString *foo = @"bar";
NSInteger answer = 42;
answer += 9;
answer++;
answer = 40 + 2;
```
The `++`, `--`, etc are preferred to be after the variable instead of before to
be consistent with other operators. Operators separated should always be
surrounded by spaces unless there is only one operand.

## <a name="types"></a>Types

`NSInteger` and `NSUInteger` should be used instead of `int`, `long`, etc per
Apple's best practices and 64-bit safety. `CGFloat` is preferred over `float`
for the same reasons. This future proofs code for 64-bit platforms.

All Apple types should be used over primitive ones. For example, if you are
working with time intervals, use `NSTimeInterval` instead of double even though
it is synonymous. This is considered best practice and makes for clearer code.

Find more informations about these types following these links:
 * [Foundation Types & Collections](https://developer.apple.com/library/ios/documentation/cocoa/conceptual/ProgrammingWithObjectiveC/FoundationTypesandCollections/FoundationTypesandCollections.html)
 * [CGGeometry Reference link to CGFloat](https://developer.apple.com/library/ios/documentation/GraphicsImaging/Reference/CGGeometry/Reference/reference.html#//apple_ref/doc/uid/TP30000955-CH2g-CJBBHACB)
 * [Foundation Data Types](https://developer.apple.com/library/ios/documentation/Cocoa/Reference/Foundation/Miscellaneous/Foundation_DataTypes/Reference/reference.html)

## <a name="code-organization"></a>Code Organization

Use `#pragma mark -` to categorize methods in functional groupings and
protocol/delegate implementations following this general structure.

```objc
#pragma mark - Lifecycle

- (instancetype)init {}
- (void)dealloc {}
- (void)viewDidLoad {}
- (void)viewWillAppear:(BOOL)animated {}
- (void)didReceiveMemoryWarning {}

#pragma mark - Custom Accessors

- (void)setCustomProperty:(id)value {}
- (id)customProperty {}

#pragma mark - IBActions

- (IBAction)submitData:(id)sender {}

#pragma mark - Public

- (void)publicMethod {}

#pragma mark - Private

- (void)privateMethod {}

#pragma mark - Protocol conformance
#pragma mark - UITextFieldDelegate
#pragma mark - UITableViewDataSource
#pragma mark - UITableViewDelegate

#pragma mark - NSCopying

- (id)copyWithZone:(NSZone *)zone {}

#pragma mark - NSObject

- (NSString *)description {}
```

## <a name="spacing"></a>Spacing

* I use 2 spaces indentation (can be set in Xcode Preferences).
* Every file ends with a newline (better diff display/scoll to end of file).

* Braces for control statements (`if`/`else`/`switch`/`while` etc.) always
open on the same line as the statement but close on a new line.
* Method braces are placed on a newline (in order to avoid copying the opening
brace of a method I'd like to expose in my header file).

**Preferred:**
```objc
- (void)doSomething
{
  if (user.isHappy) {
    //Do something
  } else {
    //Do something else
  }
}
```

**Not Preferred:**
```objc
- (void)doSomething{
  if (user.isHappy)
  {
      //Do something
  }
  else {
      //Do something else
  }
}
```

* There should be exactly one blank line between methods to aid in visual
clarity and organization. Whitespace within methods should separate
functionality, but often there should probably be new methods.
* Prefer using auto-synthesis. But if necessary, `@synthesize` and `@dynamic`
should each be declared on new lines in the implementation.
* Colon-aligning method invocation should often be avoided.  There are cases
where a method signature may have >= 3 colons and colon-aligning makes the code
more readable. Please do **NOT** however colon align methods containing blocks
because Xcode's indenting makes it illegible.
* Make liberal use of vertical whitespace to divide code into logical chunks.


**Preferred:**

```objc
// blocks are easily readable
[UIView animateWithDuration:1.0 animations:^{
  // something
} completion:^(BOOL finished) {
  // something
}];
```

**Not Preferred:**

```objc
// colon-aligning makes the block indentation hard to read
[UIView animateWithDuration:1.0
                 animations:^{
                     // something
                 }
                 completion:^(BOOL finished) {
                     // something
                 }];
```

## <a name="comments"></a>Comments

When they are needed, comments should be used to explain **why** a particular
piece of code does something. Any comments that are used must be kept
up-to-date or deleted.

Block comments should generally be avoided, as code should be as
self-documenting as possible, with only the need for intermittent, few-line
explanations. *Exception: This does not apply to those comments used to
generate documentation.*

## <a name="naming"></a>Naming

Apple naming conventions should be adhered to wherever possible, especially
those related to [memory management rules](https://developer.apple.com/library/mac/#documentation/Cocoa/Conceptual/MemoryMgmt/Articles/MemoryMgmt.html) ([NARC](http://stackoverflow.com/a/2865194/340508)).

Long, descriptive method and variable names are good.

**Preferred:**

```objc
UIButton *settingsButton;
```

**Not Preferred:**

```objc
UIButton *setBut;
```

A three letter prefix (e.g. MRC) should always be used for class names and
constants, however may be omitted for Core Data entity names. Constants should
be camel-case with all words capitalized and prefixed by the related class name
for clarity. Two letter prefix is used by Apple and you do not want your work to
collide with Apple future Framework/Classes.


Constants should be camel-case with all words capitalized and prefixed by the
related class name for clarity.

**Preferred:**

```objc
static NSTimeInterval const MRCHomeViewControllerFadeAnimationDuration = 0.3;
```

**Not Preferred:**

```objc
static NSTimeInterval const fadetime = 0.3;
```

Properties should be camel-case with the leading word being lowercase. Use
auto-synthesis for properties rather than manual @synthesize statements unless
you have good reason.

**Preferred:**

```objc
@property (strong, nonatomic) NSString *descriptiveVariableName;
```

**Not Preferred:**

```objc
id varnm;
```

### <a name="underscores"></a>Underscores

When using properties, instance variables should always be accessed and
mutated using `self.`. This means that all properties will be visually distinct,
as they will all be prefaced with `self.`.

An exception to this: inside initializers, the backing instance variable (i.e.
_variableName) should be used directly to avoid any potential side effects of
the getters/setters.

Local variables should not contain underscores.

## <a name="methods"></a>Methods

In method signatures, there should be a space after the method type (-/+
symbol). There should be a space between the method segments (matching Apple's
style). Always include a keyword and be descriptive with the word before the
argument which describes the argument.

The usage of the word "and" is reserved. It should not be used for multiple
parameters as illustrated in the `initWithWidth:height:` example below.

**Preferred:**
```objc
- (void)setExampleText:(NSString *)text image:(UIImage *)image;
- (void)sendAction:(SEL)aSelector to:(id)anObject forAllCells:(BOOL)flag;
- (id)viewWithTag:(NSInteger)tag;
- (instancetype)initWithWidth:(CGFloat)width height:(CGFloat)height;
```

**Not Preferred:**

```objc
-(void)setT:(NSString *)text i:(UIImage *)image;
- (void)sendAction:(SEL)aSelector :(id)anObject :(BOOL)flag;
- (id)taggedView:(NSInteger)tag;
- (instancetype)initWithWidth:(CGFloat)width andHeight:(CGFloat)height;
- (instancetype)initWith:(int)width and:(int)height;  // Never do this.
```

## <a name="variables"></a>Variables

Variables should be named as descriptively as possible. Single letter variable
names should be avoided except in `for()` loops.

Asterisks indicating pointers belong with the variable, e.g., `NSString *text`
not `NSString* text` or `NSString * text`, except in the case of constants.

[Private properties](#private-properties) should be used in place of instance
variables whenever possible. Although using instance variables is a valid way
of doing things, by agreeing to prefer properties our code will be more
consistent.

Direct access to instance variables that 'back' properties should be avoided
except in initializer methods (`init`, `initWithCoder:`, etc…), `dealloc`
methods and within custom setters and getters. For more information on using
Accessor Methods in Initializer Methods and dealloc, see [here](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/MemoryMgmt/Articles/mmPractical.html#//apple_ref/doc/uid/TP40004447-SW6).

**Preferred:**

```objc
@interface MRCTutorial : NSObject

@property (strong, nonatomic) NSString *tutorialName;

@end
```

**Not Preferred:**

```objc
@interface MRCTutorial : NSObject {
  NSString *tutorialName;
}
```


## <a name="property-attributes"></a>Property Attributes

Property attributes should be explicitly listed, and will help new programmers
when reading the code. The order of properties should be storage then atomicity,
which is consistent with automatically generated code when connecting UI
elements from Interface Builder.

**Preferred:**

```objc
@property (weak, nonatomic) IBOutlet UIView *containerView;
@property (strong, nonatomic) NSString *tutorialName;
```

**Not Preferred:**

```objc
@property (nonatomic, weak) IBOutlet UIView *containerView;
@property (nonatomic) NSString *tutorialName;
```

Properties with mutable counterparts (e.g. NSString) should prefer `copy`
instead of `strong`. Why? Even if you declared a property as `NSString` somebody
might pass in an instance of an `NSMutableString` and then change it without you
noticing that.  

**Preferred:**

```objc
@property (copy, nonatomic) NSString *tutorialName;
```

**Not Preferred:**

```objc
@property (strong, nonatomic) NSString *tutorialName;
```

## <a name="dot-notation-syntax"></a>Dot-Notation Syntax

Dot syntax is purely a convenient wrapper around accessor method calls. When you
use dot syntax, the property is still accessed or changed using getter and
setter methods.  Read more [here](https://developer.apple.com/library/ios/documentation/cocoa/conceptual/ProgrammingWithObjectiveC/EncapsulatingData/EncapsulatingData.html)

Dot-notation should **always** be used for accessing and mutating properties,
as it makes code more concise. Bracket notation is preferred in all other
instances.

**Preferred:**
```objc
NSInteger arrayCount = [self.array count];
view.backgroundColor = [UIColor orangeColor];
[UIApplication sharedApplication].delegate;
```

**Not Preferred:**
```objc
NSInteger arrayCount = self.array.count;
[view setBackgroundColor:[UIColor orangeColor]];
UIApplication.sharedApplication.delegate;
```

## <a name="literals"></a>Literals

`NSString`, `NSDictionary`, `NSArray`, and `NSNumber` literals should be used
whenever creating immutable instances of those objects. Pay special care that
`nil` values can not be passed into `NSArray` and `NSDictionary` literals, as
this will cause a crash.

**Preferred:**

```objc
NSArray *names = @[@"Brian", @"Matt", @"Chris", @"Alex", @"Steve", @"Paul"];
NSDictionary *productManagers = @{@"iPhone": @"Kate", @"iPad": @"Kamal", @"Mobile Web": @"Bill"};
NSNumber *shouldUseLiterals = @YES;
NSNumber *buildingStreetNumber = @10018;
```

**Not Preferred:**

```objc
NSArray *names = [NSArray arrayWithObjects:@"Brian", @"Matt", @"Chris", @"Alex", @"Steve", @"Paul", nil];
NSDictionary *productManagers = [NSDictionary dictionaryWithObjectsAndKeys: @"Kate", @"iPhone", @"Kamal", @"iPad", @"Bill", @"Mobile Web", nil];
NSNumber *shouldUseLiterals = [NSNumber numberWithBool:YES];
NSNumber *buildingStreetNumber = [NSNumber numberWithInteger:10018];
```

## <a name="constants"></a>Constants

Constants are preferred over in-line string literals or numbers, as they allow
for easy reproduction of commonly used variables and can be quickly changed
without the need for find and replace. Constants should be declared as `static`
constants and not `#define`s unless explicitly being used as a macro.

**Preferred:**

```objc
static NSString * const kMRCRealName = @"Florian PETIT";

static CGFloat const kMRCImageThumbnailHeight = 50.0;
```

**Not Preferred:**

```objc
#define RealName @"Florian PETIT"

#define thumbnailHeight 2
```

## <a name="enumerated-types"></a>Enumerated Types

When using `enum`s, it is recommended to use the new fixed underlying type
specification because it has stronger type checking and code completion. The
SDK now includes a macro to facilitate and encourage use of fixed underlying
types: `NS_ENUM()`

**For Example:**

```objc
typedef NS_ENUM(NSInteger, MRCLeftMenuTopItemType) {
  MRCLeftMenuTopItemMain,
  MRCLeftMenuTopItemShows,
  MRCLeftMenuTopItemSchedule
};
```

You can also make explicit value assignments (showing older k-style constant
definition):

```objc
typedef NS_ENUM(NSInteger, MRCGlobalConstants) {
  MRCPinSizeMin = 1,
  MRCPinSizeMax = 5,
  MRCPinCountMin = 100,
  MRCPinCountMax = 500,
};
```

Older k-style constant definitions should be **avoided** unless writing
CoreFoundation C code (unlikely).

**Not Preferred:**

```objc
enum GlobalConstants {
  kMaxPinSize = 5,
  kMaxPinCount = 500,
};
```

## <a name="bitmasks"></a>Bitmasks

When working with bitmasks, use the `NS_OPTIONS` macro.

**For Example:**

```objc
typedef NS_OPTIONS(NSUInteger, MRCAdCategory) {
  MRCAdCategoryAutos      = 1 << 0,
  MRCAdCategoryJobs       = 1 << 1,
  MRCAdCategoryRealState  = 1 << 2,
  MRCAdCategoryTechnology = 1 << 3
};
```

## <a name="case-statements"></a>Case Statements

Braces are not required for case statements, unless enforced by the complier (if
you try to declare a new variable inside a case - remember that a case does not
declare a scope of its own). When a case contains more than one line, braces
should be added.

```objc
switch (condition) {
  case 1:
    // ...
    break;
  case 2: {
    // ...
    // Multi-line example using braces
    break;
  }
  case 3:
    // ...
    break;
  default:
    // ...
    break;
}

```

There are times when the same code can be used for multiple cases, and a
fall-through should be used. A fall-through is the removal of the 'break'
statement for a case thus allowing the flow of execution to pass to the next
case value. A fall-through should be commented for coding clarity.

```objc
switch (condition) {
  case 1:
    // ** fall-through! **
  case 2:
    // code executed for values 1 and 2
    break;
  default:
    // ...
    break;
}

```

When using an enumerated type for a switch, 'default' is not needed. For
example:

```objc
MRCLeftMenuTopItemType menuType = MRCLeftMenuTopItemMain;

switch (menuType) {
  case MRCLeftMenuTopItemMain:
    // ...
    break;
  case MRCLeftMenuTopItemShows:
    // ...
    break;
  case MRCLeftMenuTopItemSchedule:
    // ...
    break;
}
```


## <a name="private-properties"></a>Private Properties

Private properties should be declared in class extensions (anonymous categories)
in the implementation file of a class. Named categories (such as `MRCPrivate` or
`private`) should never be used unless extending another class. The Anonymous
category can be shared/exposed for testing using the <headerfile>+Private.h file
naming convention.

**For Example:**

```objc
@interface MRCDetailViewController ()

@property (strong, nonatomic) GADBannerView *googleAdView;
@property (strong, nonatomic) ADBannerView *iAdView;
@property (strong, nonatomic) UIWebView *adXWebView;

@end
```

## <a name="image-naming"></a>Image Naming

Image names should be named consistently to preserve organization and developer
sanity. They should be named as one camel case string with a description of
their purpose, followed by the un-prefixed name of the class or property they
are customizing (if there is one), followed by a further description of color
and/or placement, and finally their state.

**For Example:**

 * `RefreshBarButtonItem` / `RefreshBarButtonItem@2x` and
 `RefreshBarButtonItemSelected` / `RefreshBarButtonItemSelected@2x`
 * `ArticleNavigationBarWhite` / `ArticleNavigationBarWhite@2x` and
 `ArticleNavigationBarBlackSelected` / `ArticleNavigationBarBlackSelected@2x`

 Images that are used for a similar purpose should be grouped in respective
 groups in an Images folder.



## <a name="booleans"></a>Booleans

Objective-C uses `YES` and `NO`.  Therefore `true` and `false` should only be
used for CoreFoundation, C or C++ code. Since `nil` resolves to `NO` it is
unnecessary to compare it in conditions. Never compare something directly to
`YES`, because `YES` is defined to 1 and a `BOOL` can be up to 8 bits.

This allows for more consistency across files and greater visual clarity.

**Preferred:**

```objc
if (someObject) {}
if (![anotherObject boolValue]) {}
```

**Not Preferred:**

```objc
if (someObject == nil) {}
if ([anotherObject boolValue] == NO) {}
if (isAwesome == YES) {} // Never do this.
if (isAwesome == true) {} // Never do this.
```

If the name of a `BOOL` property is expressed as an adjective, the property can
omit the “is” prefix but specifies the conventional name for the get accessor,
for example:

```objc
@property (assign, getter=isEditable) BOOL editable;
```
Text and example taken from the [Cocoa Naming Guidelines](https://developer.apple.com/library/mac/#documentation/Cocoa/Conceptual/CodingGuidelines/Articles/NamingIvarsAndTypes.html#//apple_ref/doc/uid/20001284-BAJGIIJE).

## <a name="conditionals"></a>Conditionals

Conditional bodies should always use braces even when a conditional body could
be written without braces (e.g., it is one line only) to prevent errors. These
errors include adding a second line and expecting it to be part of the
if-statement. Another, [even more dangerous defect](http://programmers.stackexchange.com/a/16530)
may happen where the line "inside" the if-statement is commented out, and the
next line unwittingly becomes part of the if-statement. In addition, this style
is more consistent with all other conditionals, and therefore more easily
scannable.

**Preferred:**
```objc
if (!error) {
  return success;
}
```

**Not Preferred:**
```objc
if (!error)
  return success;
```

or

```objc
if (!error) return success;
```

### <a name="ternary-operator"></a>Ternary Operator

The Ternary operator, `?:` , should only be used when it increases clarity or
code neatness. A single condition is usually all that should be evaluated.
Evaluating multiple conditions is usually more understandable as an `if`
statement, or refactored into instance variables. In general, the best use of
the ternary operator is during assignment of a variable and deciding which value
to use.

Non-boolean variables should be compared against something, and parentheses are
added for improved readability.  If the variable being compared is a boolean
type, then no parentheses are needed.

**Preferred:**
```objc
NSInteger value = 5;
result = (value != 0) ? x : y;

BOOL isHorizontal = YES;
result = isHorizontal ? x : y;
```

**Not Preferred:**
```objc
result = a > b ? x = c > d ? c : d : y;
```

## <a name="init-methods"></a>Init Methods

Init methods should follow the convention provided by Apple's generated code
template. A return type of 'instancetype' should also be used instead of 'id'.

```objc
- (instancetype)init
{
  self = [super init];
  if (self) {
    // ...
  }
  return self;
}
```

See [Class Constructor Methods](#class-constructor-methods) for link to article
on instancetype.

## <a name="class-constructor-methods"></a>Class Constructor Methods

Where class constructor methods are used, these should always return type of
'instancetype' and never 'id'. This ensures the compiler correctly infers the
result type.

```objc
@interface Airplane
+ (instancetype)airplaneWithType:(MRCAirplaneType)type;
@end
```

More information on instancetype can be found on [NSHipster.com](http://nshipster.com/instancetype/).

## <a name="inheritance-super"></a> Inheritance & Super

When should you call super in an overridden method ?
The usual rule of thumb is that when you are overriding a method that does some
kind of initialization/setup, you should call super first and then do your
custom stuff. And when you override some kind of teardown method, you should
call super last.

Setup methods are: `init...`, `viewWillAppear`, `viewDidAppear`, `setUp`.

Teardown methods are: `dealloc`, `viewDidUnload`, `viewWillDisappear`,
`tearDown`.


**For Example:**

```objc
- (void)viewDidLoad
{
  [super viewDidLoad];
  // Documentation does not specify when this should be
  // called, only that it has to be called at "some point"
  // ...
}
```

## <a name="cgrect-functions"></a>CGRect Functions

When accessing the `x`, `y`, `width`, or `height` of a `CGRect`, always use the
[`CGGeometry` functions](http://developer.apple.com/library/ios/#documentation/graphicsimaging/reference/CGGeometry/Reference/reference.html)
instead of direct struct member access. From Apple's `CGGeometry` reference:

> All functions described in this reference that take CGRect data structures as
inputs implicitly standardize those rectangles before calculating their results.
For this reason, your applications should avoid directly reading and writing the
data stored in the CGRect data structure. Instead, use the functions described
here to manipulate rectangles and to retrieve their characteristics.

**Preferred:**

```objc
CGRect frame = self.view.frame;

CGFloat x = CGRectGetMinX(frame);
CGFloat y = CGRectGetMinY(frame);
CGFloat width = CGRectGetWidth(frame);
CGFloat height = CGRectGetHeight(frame);
CGRect frame = CGRectMake(0.0, 0.0, width, height);
```

**Not Preferred:**

```objc
CGRect frame = self.view.frame;

CGFloat x = frame.origin.x;
CGFloat y = frame.origin.y;
CGFloat width = frame.size.width;
CGFloat height = frame.size.height;
CGRect frame = (CGRect){ .origin = CGPointZero, .size = frame.size };
```

## <a name="golden-path"></a>Golden Path

When coding with conditionals, the left hand margin of the code should be the
"golden" or "happy" path. That is, don't nest `if` statements. Multiple return
 statements are OK.

**Preferred:**

```objc
- (void)someMethod
{
  if (![someOther boolValue]) {
  return;
  }

  //Do something important
}
```

**Not Preferred:**

```objc
- (void)someMethod {
  if ([someOther boolValue]) {
    //Do something important
  }
}
```

## <a name="error-handling"></a>Exceptions & Error handling

* Don't use exceptions for flow control.
* Use exceptions only to indicate programmer error.

* When methods return an error parameter by reference, switch on the returned
value, not the error variable.

**Preferred:**
```objc
NSError *error;
if (![self trySomethingWithError:&error]) {
  // Handle Error
}
```

**Not Preferred:**
```objc
NSError *error;
[self trySomethingWithError:&error];
if (error) {
  // Handle Error
}
```

Some of Apple’s APIs write garbage values to the error parameter (if non-NULL)
in successful cases, so switching on the error can cause false negatives (and
subsequently crash).


## <a name="singletons"></a>Singletons

Singleton objects should use a thread-safe pattern for creating their shared
instance.
```objc
+ (instancetype)sharedInstance {
  static id sharedInstance = nil;

  static dispatch_once_t onceToken;
  dispatch_once(&onceToken, ^{
    sharedInstance = [[self alloc] init];
  });

  return sharedInstance;
}
```
This will prevent [possible and sometimes prolific crashes](http://cocoasamurai.blogspot.com/2011/04/singletons-your-doing-them-wrong.html).


## <a name="line-breaks"></a>Line Breaks

Line breaks should be applied according the 100 column rule I'm using in Xcode
(a column guide can be set in preferences). Pretty neat to avoid wrapping when
using the splitted Xcode's Assistant Editor.

For example:
```objc
self.productsRequest = [[SKProductsRequest alloc] initWithProductIdentifiers:productIdentifiers];
```
A long line of code like this should be carried on to the second line adhering
to this style guide's Spacing section (two spaces).
```objc
self.productsRequest = [[SKProductsRequest alloc]
  initWithProductIdentifiers:productIdentifiers];
```

## <a name="imports"></a>Imports

Always use `@class` whenever possible in header files instead of `#import` since
it has a slight compile time performance boost. (In fact the Type is know but
the header is not imported so you'll be reducing compiler overhead)

From the [Objective-C Programming Guide](http://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/ObjectiveC/ObjC.pdf) (Page 38):

> The @class directive minimizes the amount of code seen by the compiler and
linker, and is therefore the simplest way to give a forward declaration of a
class name. Being simple, it avoids potential problems that may come with
importing files that import still other files. For example, if one class
declares a statically typed instance variable of another class, and their two
interface files import each other, neither class may compile correctly.

Apple introduced a new `@import` directive, starting with Xcode 5 you can enable
module feature from your target build settings and enable the Link Frameworks
Automatically option to automatically import frameworks 'on-the-fly' while using
`@import`, this directive also reduces compiler overhead. Have a look [Here](http://stackoverflow.com/a/18947634)



## <a name="xcode-project"></a>Xcode project

The physical files should be kept in sync with the Xcode project files in order
to avoid file sprawl. Any Xcode groups created should be reflected by folders in
the filesystem. Code should be grouped not only by type, but also by feature for
greater clarity.

When possible, always turn on "Treat Warnings as Errors" in the target's Build
Settings and enable as many [additional warnings](http://boredzo.org/blog/archives/2009-11-07/warnings)
as possible. If you need to ignore a specific warning, use [Clang's pragma feature](http://clang.llvm.org/docs/UsersManual.html#controlling-diagnostics-via-pragmas).

---

Other Style guides that may influence your coding style:

  - [NY-Times Objective-C Style Guide](https://github.com/MRCimes/objective-c-style-guide)
  - [Apple Coding Guidelines for Cocoa](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/CodingGuidelines/CodingGuidelines.html)
  - [Apple Programming with Objective-C](https://developer.apple.com/library/mac/documentation/cocoa/conceptual/ProgrammingWithObjectiveC/Conventions/Conventions.html)
  - [Apple Avoid Category Method Name Clashes](https://developer.apple.com/library/mac/documentation/cocoa/conceptual/ProgrammingWithObjectiveC/CustomizingExistingClasses/CustomizingExistingClasses.html#//apple_ref/doc/uid/TP40011210-CH6-SW2)
  - [Google Objective-C Style Guide](http://google-styleguide.googlecode.com/svn/trunk/objcguide.xml)
  - [Raywenderlich Style Guide](https://github.com/raywenderlich/objective-c-style-guide)
  - [Github Objective-C Conventions](https://github.com/github/objective-c-conventions)
  - [Sam Soffes Style Guide](https://gist.github.com/soffes/812796)
  - [Luke Redpath Style Guide](http://lukeredpath.co.uk/blog/2011/06/28/my-objective-c-style-guide/)
