Description
===========

A lightweight dependency injection framework for Objective-C. With support for iOS and MacOS X.

Synopsis
========

### Basic Usage

A class can be registered with objection using the macros *objection_register* or *objection_register_singleton*. The *objection_requires* macro can be used to declare what dependencies objection should provide to all instances it creates of that class. *objection_requires* can be used safely with inheritance.

#### Example

      @class Engine, Brakes;
    
      @interface Car : NSObject
      {
        Engine *engine;
        Brakes *brakes;
        BOOL awake;  
      }

      // Will be filled in by objection
      @property(nonatomic, retain) Engine *engine;
      // Will be filled in by objection
      @property(nonatomic, retain) Brakes *brakes;
      @property(nonatomic) BOOL awake;
    
      @implementation Car
      objection_register(@"Car")
      objection_requires(@"engine", @"brakes")
      @synthesize engine, brakes, awake;
      @end

### Registering Objects

Objection supports associating an object outside the context of Objection with a class.

### Example
      - (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions {    
        [ObjectionInjector 
          registerObject:[UIApplication sharedApplication] 
          forClass:[UIApplication class]];  
      }

### Instance Creation Notification

If an object is interested in knowing when it has been fully instantiated by objection it can implement the method
*awakeFromObjection*.

#### Example
      @implementation Car
      //...
      objection_register_singleton(@"Car")
        - (void)awakeFromObjection {
          awake = YES;
        }
      @end  
      

### Fetching Objects from Objection

      - (void)someMethod {
        id car = [ObjectionInjector getObject:[Car class]];
      }

### TODO

* Migrate to using a module specific context rather than a shared global context (similar to Guice)

          @implementation MyModule
            - (void) configure {
              [self bind:[UIApplication sharedApplication] toClass:[UIApplication class]];
            }
          @end
    
          - (void)forExample {
            ObjectionContext context = [Objection getContext:aModule];
            id object = [context getObject:[MyObject class]];
          }

Installation
=======

### iOS

1. git clone git://github.com/atomicobject/objection.git
2. Open Objection.xcodeproj
3. Select Objection-iPhone target
4. Select Release Configuration.
5. Build

#### Include framework
    #import <Objection-iPhone/ObjectionInjector.h>

### MacOS X

1. git clone git://github.com/atomicobject/objection.git
2. Open Objection.xcodeproj
3. Select Objection target
4. Select Release Configuration.
5. Build

#### Include framework
    #import <Objection/ObjectionInjector.h>

Requirements
============

* MacOS X 10.6 >
* iOS 3.0 >

