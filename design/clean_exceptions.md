# Clean Exceptions

While working on Relic, I encountered an issue trying to design "clean" layers.
Her's what what a few kayers look like in my application
``Viewmodel <- Repository <- Deserialization``

The *Viewmodel* classes are aware of the *Repository* classes which have apis defined by a set of interfaces, but not the other way around. Similarily, the *Repository* classes are aware of the *Deserialization*. Following these steps, the ViewModel doesn't have to worry about the implementation of the repository and the fact that the deserialization class even exists.

This looked pretty good until I started working on adding and refining exception handling.

Let me give an example which will make things a bit clearer:

I use an external `JsonParser` library in my Deserialization classes. When my Repository interacts with the deserialization classes, the details of the deserialization process are hidden. There is no concept of `JsonParser`, all the repository class knows is that it calls a deserialize method with some arguments to get some sort of result. But that's not the case if the `JsonParser` happens to throw some sort of exception in the deserialization process.

Our Deserialization class doesn't know what to do with the exception, so we allow it to bubble up. All of a sudden, our Repository (or Viewmodel) class has the responsibility of handling an implementation specific exception it should not be aware of.

My current (and probably naive) solution was to continue following the same clean architecture guidelines I had already been following. I created a set of exceptions owned by each layer. It looks something like this:

```kotlin
// base exception defined for my application
class RelicException ( ... ) : Exception ( ... ) { ... }

// exceptions that can be thrown by repository
sealed class RepoExceptions  ( ... ) : RelicException ( ... ) { ... }

// exceptions that can be thrown by deserializer
sealed class DeserializationExceptions  ( ... ) : RelicException ( ... ) { ... }
```

To me, this is the "cleanest" approach, but it requires a great deal of work since we now have to map implementation specific exceptions to exceptions owned by each layer. It's cumbersome and *feels* wrong whenever I have to do it.

I'm still exploring other options right now, but I'll leave this here for now

To be continued...