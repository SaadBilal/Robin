# Robin

[![API](https://img.shields.io/badge/API-15%2B-brightgreen.svg?style=flat)](https://android-arsenal.com/api?level=15)
[![Open Source Love](https://badges.frapsoft.com/os/v1/open-source.svg?v=102)](https://opensource.org/licenses/Apache-2.0)
[![License](https://img.shields.io/badge/license-Apache%202.0-blue.svg)](https://github.com/balsikandar/CrashReporter/blob/master/LICENSE)

## Robin is a library to log Bundle data passed between Activities and fragments.
 
### It's hard to reproduce a bug that's why we should log data that can reduce human efforts. 

One of my friend said this and it's totally true. That's why i always try to log essential data.

## Bundle
Definition: A mapping from String values to various Parcelable types. Bundle is generally used for passing data between various activities of android. 

I have spent good amount of time to debug these at times, depending on code quality i had to work on. Sometimes even tester need to see what **id** or **values** we're passing to open a given page and then i had to add logs for them.
So i thought let's do something about this.


## Robin In Action APIs
- Log Bundle data passed between Activity and Fragments.
- A Callback method to send screen views events to Google Analytics or other clients
- A Callback method for Activity/Fragment lifecycle which can be used to retrace user activities before crashes
- An API to transform Intent or Bundle data as HashMap (key-value)

##### This library doesn't requires any permission or root access

## To use this in your app

Use below dependency to add it in your app

```
compile 'com.balsikandar.android:robin:'
```

If you only want to use it in your debug app then use debugCompile dependency

```
debugCompile 'com.balsikandar.android:crashreporter:'
```

### Robin in action

Initialise this library in your Application class using

```
Robin.start(this)
```

If you only want to use bundle data logging and disable other callbacks then initialise Robin as

```
Robin.start(this, false)
```

### The other moves of Robin

#### 1- Tracking screen views

```
AnalyticsClient.sendView(AppConstants.ScreenViewLabel);
```
To simplify that, you can implement **ScreenViewCallback** interface in your application class which provides below callback method

```
    @Override
    public void onScreenShown(String className, String customScreenView) {
        
    }
```

**className**: is actual Activity/Fragment name which you can use to log screenName on your analytics client. 

**customScreenView**:  but a lot of times we need to log custom screenNames. To do that you can declare a global variable named **screenView** in your Activity/Fragment classes and it'll be returned as `customScreenView` in **onScreenShown** callback.

Define a global variable **screenView** in your Activity/Fragment with label value
 
 ```
 private String screenView = "RobinSampleHomePage";
 ```

#### 2- Retrace user actions before a Crash

At times we get stacktrace log where we have no idea of where it's originated and it's not even possible to figure out in which class crash occurred. 

Whatever you log with `Crashlytics.log(msg)` gets associated with the crashes and is viewable in crashlytics dashboard.
These log can really help in narrowing down origin of a crash and Activity/Fragment lifecycle callback methods can come in handy.

To do this, implement **LifeCycleCallbacks** interface in your application class which provides below callback method

```
    @Override
    public void breadCrumps(String name, String callback) {
        
    }
```
 
 **name**: is Activity/Fragment class name,
 **callback**: is lifecycle callback method name

#### 3- Transform Bundle data to HashMap

Use Below APIs to get bundle data as HashMap key-value pairs
 
 ```
    Robin.getDataAsMap(intent)
     
    Robin.getDataAsMap(bundle)
 ```
 
 you can also call below APIs explicitly to log bundle or intent data
 
 ```
    Robin.logData(intent, "tagName");
    
    Robin.logData(bundle, "tagName");
 ```
 
 **tagName**- provide this field to filter log in LogCat and if you don't provide this tag **Robin/** will be used.
