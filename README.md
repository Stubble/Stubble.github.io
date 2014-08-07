stubble [![Build Status](https://travis-ci.org/Stubble/stubble.svg)](https://travis-ci.org/Stubble/stubble) [![Cocoa Pod](http://cocoapod-badges.herokuapp.com/p/stubble/badge.svg)](http://cocoapods.org/?q=stubble) [![Cocoa Pod](http://cocoapod-badges.herokuapp.com/v/stubble/badge.svg)](http://cocoapods.org/?q=stubble)
=======

Mocking Objective-C since [NSDate dateWithTimeIntervalSinceReferenceDate:408029584]

Using stubble
=======

Project Setup
-------
Stubble can be imported as a subproject in Xcode or as a .framework. If imported as a subproject, make sure you update the Header Search Paths in the build settings for your Test target, and to update the Link Binary With Libraries in Build Phases.

In the build settings for your Test target, under `OtherLinkerFlags` add `-ObjC` and `-all_load`.

Mocking
------

Mocking a class
    
    mock([ClassToMock class])
    or
    mock(ClassToMock.class)

Mocking a protocol
    
    mock(@protocol(ProtocolToMock))

Resetting a mock

    resetMock(MockedObject)

#### Note with ESCObservable
If you are using ESCObservable and intend to fire events from your mock object, you'll need to declare your mock objects as

    ClassToMock<ESCObservableInternal> *mockObject = mock(ClassToMock);
    [mockObject escRegisterObserverProtocol:@protocol(ClassToMockObserver)];

Stubbing
------

Stubbing method calls on a mock

    [when([MockedObject methodToStub]) thenReturn:@"string"]
    or 
    (for BOOL)
    [when([MockedObject isFireflyTheBestScienceFictionShowEver]) thenReturn:@YES]
    or 
    (for NSInteger)
    [when([MockedObject getInteger]) thenReturn:@1337]

#### Note with ESCObservable
You can do something like this

    [mockObject.escNotifier notificationMethod]

Verifying
------

Verify method called on mock

    verifyCalled([MockedObject methodToStub])
    or
    verify([MockedObject methodToStub])

Verify method never called
    
    verifyNever([MockedObject methodToStub])
    or
    verifyTimes(never(), [MockedObject methodToStub])

Verify method called a number of times

    verifyTimes(times(#), [MockedObject methodToStub])

Verify method called at least a number of times

    verifyTimes(atLeast(#), [MockedObject methodToStub])

Verify method called at least once (equivalent to verifyCalled([MockedObject methodToStub]))

    verifyTimes(atLeastOnce(), [MockedObject methodToStub])

Verify method called a number of times within limits

    verifyTimes(between(#,#), [MockedObject methodToStub])

Verify method called only once (equivalent to verifyTimes(times(1), [MockedObject methodToStub]))

    verifyTimes(once(), [MockedObject methodToStub])
    
Verify methods called in order

    SBLOrderToken *orderToken = orderToken();
    verifyInOrder(orderToken, [MockedObjectA methodToStub]);
    verifyInOrder(orderToken, [MockedObjectB methodToStub]);
    
    verifyTimesInOrder(timesMatcher, orderToken, [MockedObject methodToStub])
    
Verify no interactions with a mock

    verifyNoInteractions(MockedObject)

Times Matchers
------

A number of times

    times(times)

At least a number of times

    atLeast(times)

Never
    
    never()

Once
    
    once()

At least once
    
    atLeastOnce()

Between

    between(atLeast, atMost)

Argument Matchers
------

Any

    any()
    or
    any(BOOL)
    or 
    any(NSInteger)

Capturing Arguments
------

Capture

    capture(CaptorReference)

For example

    NSString *element1Id = nil;
    verifyCalled([mockObject integerParameter:capture(&element1Id)]);
