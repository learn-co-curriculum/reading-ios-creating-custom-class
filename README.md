# Creating A Custom Class

## Objectives

1. Learn the value of grouping classes into libraries and the purpose behind class prefixes in Objective-C.
2. Recognize when to create a custom class.
3. Generate a new pair of class files (`.h`/`.m`) in Xcode.
4. Review the generated files for a new custom class.
5. Add properties and compose methods on a custom class.
6. Use `#import` to give another class knowledge of a custom class.
7. Interact with an instance of a custom class from another class file.
8. Add a custom class as a property on another class, use the `self.` keyword to interact with it.

## Class Libraries

In Object-Oriented Programming (OOP), the library of available objects is as much a part of the language as the syntax itself. Without a library of objects to "instantiate" (i.e. to create on instance of), an object-oriented language would be like having grammar but no vocabulary—there exists the structure of a language, but nothing to actually communicate with.

In Objective-C, individual libraries are called "frameworks". So far, you've worked exclusively with objects from the Core Foundation library. The vast majority of foundation framework classes are prepended with the `NS` prefix which stands for "NeXTSTEP". This was the computer product engineered by the "NeXT" company who developed the Core Foundation library in the late 1980s, building it with the [Objective-C][objective-c] language syntax created by [Brad Cox][brad_cox] in the early 1980s.

[objective-c]: https://en.wikipedia.org/wiki/Objective-C
[brad_cox]: https://en.wikipedia.org/wiki/Brad_Cox

Because the number of objects available within the language as a whole is staggering, they are collected into separate libraries (or "frameworks") based upon their intended usages. Some of the commonly-used Apple frameworks that you'll see include:

| Prefix | Framework       | Purpose |
|:------:|:----------------|:--------|
| `NS`   | Core Foundation | Basic Objective-C objects.
| `UI`   | UI Kit          | "User Interface" objects for building views and buttons. |
| `CA`   | Core Animation  | Drawing 2D animations based upon key frames. |
| `CG`   | Core Graphics   | Rendering 2D graphic effects. |
| `SK`   | Scene Kit       | Rendering 3D objects and effects. |
| `CL`   | Core Location   | Handling location data on a GPS-capable device. |
| `MK`   | Map Kit         | Integrating with Apple Maps. |

Each of these libraries contains hundreds or even thousands of prefabricated class files. Because many of them are very large and have rather specific purposes, any library other than Core Foundation is not typically "imported" (or loaded) into a class file until the additional functionality of that supplemental library is required.

### Custom Class Prefixing

You may have noticed the `FIS` prefix on the `FISAppDelegate` class in previous labs; these are the initials for "**F**lat**i**ron **S**chool", and is the customized prefix that we use in all of our Objective-C labs. It's best practice to prefix custom classes in order to disambiguate them from the classes in other libraries. While it's unlikely that you'd create a custom class that coincides with a class in an Apple framework, name collisions become a very real possibility when importing third party frameworks found in the Open Source community (i.e. via CocoaPods like the Specta and Expecta testing frameworks).

Imagine that you're building a messaging app. Without class prefixing, you might create your own custom `Message` class. Now, if you decide to import another library that didn't use a class prefix either and which also includes a `Message` class, your application won't be able to distinguish between the two causing a slew of problems. However, if both you and the third party developer whose library you imported uniquely prefixed both `Message` classes (e.g. `FISMessage` and `JBMessage`), then the filenames would be unique. Furthermore, you might find yourself working with two libraries which both include a `Message` class, so it's similarly important that the classes are distinct:

[JBMessage.h](https://github.com/josipbernat/JBMessage/blob/master/JBMessage/JBMessage/JBMessage.h)  
[TSMessage.h](https://github.com/KrauseFx/TSMessages/blob/master/Pod/Classes/TSMessage.h)

Prefixes are important in Objective-C so build the habit of using them early. Because of the testing suites, you'll have to keep to the `FIS` prefix when working on labs, but you should decide upon your own personal three-letter prefix for yourself when working on your own side projects, and a project- or group-specific prefix when working with a team.

**Note:** *Apple reserves the authority to use any two-letter prefix for new libraries at future dates. For this reason, they suggest that third-party developers choose three-letter prefixes because there is no chance that Apple will create a future library with that exact prefix. This guideline is not always followed in the Open Source community, but it is considered best practice to use three-letter prefixes.*

## A Time To Subclass

Despite the plethora of class files available to us, solving any specific problem typically requires creating at least a few custom classes to tailor-fit our code to the current situation. None of the Apple libraries include, for example, a `Warship` class, nor a `ToySoldier` class, nor a `ChessBoard` class. In order to build an application that requires a representation of one of these objects, we would have to create a custom class to hold the data (properties) and perform the behaviors (methods) that the representation requires.

Every "application program" addresses some problem or set of problems that are relevant to some aspect of life—this actually is the etymology of calling a program an "application", because it has a *real-world application*. Within the context of an application program, some custom modeling will need to be done. The [system software](https://en.wikipedia.org/wiki/System_software) that supports [application software](https://en.wikipedia.org/wiki/Application_software) cannot possibly anticipate the needs of every application: a shopping cart application will need to model a cart and an item, a music player will need to model a song and a playlist, a flight simulator will need to model an airplane and a runway. It is our job as the developer to anticipate the needs of the application presented to us and to create those customized models when writing the application program.

Creating a custom class is called "subclassing" and as a practice relies heavily on the concept of inheritance. Since we're not going to discuss the concept of inheritance until later, right now we're only going to subclass from `NSObject`—the most basic object of all of the objects; even it's name is boring.

**//Flat-fact:** *You may see the word "ponso" online; it is derived from the acronym PONSO meaning "Plain Old NSObject".*

Despite being rudimentary, this is actually quite common in programming. `NSObject` provides a (nearly) blank slate to start with when defining what properties and methods a class should contain.

## Generate A New Pair Of Class Files

To generate a new pair of class files (`.h`/`.m`) in Xcode, we can follow these steps:

1 — In the Xcode taskbar, select `File` => `New` => `File` (or press `⌘` `N`):

![](https://curriculum-content.s3.amazonaws.com/ios/ios-intro-to-objects-unit/subclass_01_select_new_file.png)

2 — Within the `iOS` : `Source` category in the template navigator, select `Cocoa Touch Class` and hit `next`: 

![](https://curriculum-content.s3.amazonaws.com/ios/ios-intro-to-objects-unit/subclass_02_cocoa_touch.png)

3 — Name the new class, in this case `FISWarship`:

![](https://curriculum-content.s3.amazonaws.com/ios/ios-intro-to-objects-unit/subclass_03_name_class.png)

4 — Define the parent class, in this case `NSObject`. Then select `Next`:

![](https://curriculum-content.s3.amazonaws.com/ios/ios-intro-to-objects-unit/subclass_04_inheritance.png)

5 — Select the target. This should remain the default selection of the application's name, and not its tests:

![](https://curriculum-content.s3.amazonaws.com/ios/ios-intro-to-objects-unit/subclass_05_targets.png)

6 — The new class files should appear in the project navigator. If they do not generate below the `FISAppDelegate` files, they can be dragged to that placement in the project navigator without harming anything:

![](https://curriculum-content.s3.amazonaws.com/ios/ios-intro-to-objects-unit/subclass_06_navigator.png)

## `#import` Dependencies

The `#import` keyword is used to give your class files knowledge of other class files. You'll notice by default that the Core Foundation has been imported in the header (`.h`) file, and that the header file has been imported into the associated implementation (`.m`) file:

```objc
// FISWarship.h

#import <Foundation/Foundation.h>

@interface FISWarship : NSObject

@end
```

```objc
// FISWarship.m

#import "FISWarship.h"

@implementation FISWarship

@end
```

You may have noticed that the library was imported using angle brackets (`<` `>`) while the individual header file was imported using double quotes (`"` `"`). This is a standard syntax for distinction: code specifically belonging to the current application (i.e. code that you wrote) is imported using double quotes (`"` `"`), while code *not* specific to the current application (i.e. code that you *didn't* write) is imported using angle brackets (`<` `>`).

**Advanced:** *Each library has its own library header file (with no associated implementation file) that simply contains all of the header files for every class in the library. So, to import the entire framework in it's entirety, the library header is used instead of individual class headers because they often have dependencies on other classes within their native library.*

So, in order for our `FISAppDelegate` class to have knowledge of the `FISWarship` class, we'll need to import the `FISWarship.h` header file into the `FISAppDelegate.m` implementation file.

```objc
// FISAppDelegate.m

#import "FISAppDelegate.h"
#import "FISWarship.h"
```

## Add Properties And Compose Methods

Now that we have our class file, we can fill it out with the properties and methods that we wish our new `FISWarship` class to have:

```objc
// FISWarship.h

#import <Foundation/Foundation.h>

@interface FISWarship : NSObject

@property (strong, nonatomic) NSString *shipName;
@property (nonatomic) NSUInteger currentSpeedInKnots;
@property (nonatomic) NSUInteger maximumSpeedInKnots;

- (void)aheadOneHalfSpeed;
- (void)fullStop;
- (void)fullSpeedAhead;

@end
```

```objc
// FISWarship.m

#import "FISWarship.h"

@implementation FISWarship

- (void)aheadOneHalfSpeed {
    self.currentSpeedInKnots = self.maximumSpeedInKnots / 2;
    NSLog(@"Ahead one half speed, aye!");
}

- (void)fullStop {
    self.currentSpeedInKnots = 0;
    NSLog(@"Full stop, aye!");
}

- (void)fullSpeedAhead {
    self.currentSpeedInKnots = self.maximumSpeedInKnots;
    NSLog(@"Full speed ahead, aye!");
}

@end
```

## Interacting With A Custom Class

Once another class has knowledge of our new custom class (i.e. has been given an `#import` statement for our new custom class), we can create an instance of our custom class:

```objc
// FISAppDelegat.m

#import "FISAppDelegate.h"

@implementation FISAppDelegate

- (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions {
    
    FISWarship *ussIowa = [[FISWarship alloc] init];
}

@end
```

Then set its properties and call its methods:

```objc
// FISAppDelegat.m

#import "FISAppDelegate.h"
#import "FISWarship.h"

@implementation FISAppDelegate

- (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions {
    
    FISWarship *ussIowa = [[FISWarship alloc] init];
    ussIowa.shipName = @"USS Iowa";
    ussIowa.maximumSpeedInKnots = 33;
    ussIowa.currentSpeedInKnots =  0;

    [ussIowa aheadOneHalfSpeed];
    NSLog(@"Speed: %lu knots", ussIowa.currentSpeedInKnots);

    [ussIowa fullStop];
    NSLog(@"Speed: %lu knots", ussIowa.currentSpeedInKnots);

    [ussIowa fullSpeedAhead];
    NSLog(@"Speed: %lu knots", ussIowa.currentSpeedInKnots);
   
    return YES;
}

@end
```

This will print:

```
Ahead one half speed, aye!
Speed: 16 knots
Full stop, aye!
Speed: 0 knots
Full speed ahead, aye!
Speed: 33 knots
```

### Custom Classes As Properties

We could also make our custom class into a property owned by `FISAppDelegate`:

```objc
// FISAppDelegate.h

#import <UIKit/UIKit.h>
#import "FISWarship.h"

@interface FISAppDelegate : UIResponder <UIApplicationDelegate>

@property (strong, nonatomic) FISWarship *ussIowa;

@end
```
Setting and accessing its properties and calling its methods would then require the `self.` keyword.

```objc
// FISAppDelegate.m

#import "FISAppDelegate.h"

@implementation FISAppDelegate

- (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions {
    
    self.ussIowa = [[FISWarship alloc] init];
    self.ussIowa.shipName = @"USS Iowa";
    self.ussIowa.maximumSpeedInKnots = 33;
    self.ussIowa.currentSpeedInKnots =  0;

    [self.ussIowa aheadOneHalfSpeed];
    NSLog(@"Speed: %lu knots", self.ussIowa.currentSpeedInKnots);
    
        [self.ussIowa fullStop];
    NSLog(@"Speed: %lu knots", self.ussIowa.currentSpeedInKnots);

    [self.ussIowa fullSpeedAhead];
    NSLog(@"Speed: %lu knots", self.ussIowa.currentSpeedInKnots);
    
    return YES;
}

@end
```

This will also print:

```
Ahead one half speed, aye!
Speed: 16 knots
Full stop, aye!
Speed: 0 knots
Full speed ahead, aye!
Speed: 33 knots
```
Now, bring us that horizon!

![](https://curriculum-content.s3.amazonaws.com/ios/ios-intro-to-objects-unit/ios-intro-to-objects-unit/drink_up_me_hearties.gif)