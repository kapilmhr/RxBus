RxBus
===========

Event bus based on RxJava and optimized for Android.

Usage
-------

We recommend obtaining the single instance of bus through injection or another appropriate mechanism.

Or get singleton like following:

```
Bus bus = BusProvider.getInstance();
```

#### Subscribing

To subscribe to an event, declare and annotate a method with @Subscribe. The method should be public and take only a single parameter.

```
@Subscribe
public void onEvent(SomeEvent event) {
    // TODO: Do something
}
```

You can also create subscription like following:

```
CustomSubscriber<SomeEvent> customSubscriber = bus.obtainSubscriber(SomeEvent.class,
    new Consumer<SomeEvent>() {
        @Override
        public void accept(SomeEvent someEvent) throws Exception {
            // TODO: Do something
        }
    })
    .withFilter(new Predicate<SomeEvent>() {
        @Override
        public boolean test(SomeEvent someEvent) throws Exception {
            return "Specific message".equals(someEvent.message);
        }
    })
    .withScheduler(Schedulers.trampoline());
```

#### Register and unregister your observer

To receive events, a class instance needs to register with the bus.

```
bus.register(this);
```

The customSubscriber also needs to register with the bus.

```
bus.registerSubscriber(this, customSubscriber);
```

Remember to also call the unregister method when appropriate.
```
bus.unregister(this);
```

#### Publishing

To publish a new event, call the post method:

```
bus.post(new SomeEvent("Message"));
```

#### Add RxBus to your project

Gradle:

Add it in your root build.gradle at the end of repositories:

``` allprojects {
		repositories {
			...
			maven { url 'https://jitpack.io' }
		}
	}

```
    
    # Add your dependency
    
``` 
 {
	        implementation 'com.github.kapilmhr:RxBus:1.0'
	}
```

ProGuard
-------

If you are using ProGuard, add the following lines to your ProGuard configuration file.

```
-keepattributes *Annotation*
-keepclassmembers class ** {
    @app.frantic.mylibrary.Subscribe public *;
}
```
