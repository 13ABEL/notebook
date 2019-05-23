# Nothing

Nothing is a compile time abstraction
explicitly indicates failure - function never completes successfully

Nothing is a subtype of every class

why `Nothing` over `Unit`?
> `Unit` is a subtype of `Any`. Makes it difficult to work with
Ex. suppose we have a function `getString() : String?`. In our use case -> we should throw an exception if calling `getString()` returns `null`.

This this is what it would look like without `Nothing`:

```kotlin
val username : String? = args.getString("username")
if (username == null) {
    throw CustomE("User doesn't exist")
} else {
    // perform operation using username
}
... rest of code
```

This is alot more concise (imo the throw is just as explicit so no advantage there)

```kotlin
val username = args.getString("username") ?: fail("User doesn't exist")
    // perform rest of operation using username
... rest of code

fun fail(message : String) : Nothing {
    throw CustomE(message)
}
```

Using `Nothing`, we can avoid the additional null check after calling `getString()`. This preserves the verbosity of failure without polluting our code with a bunch of null checks

## Sources

1. [Lorenzo Quiroli's article](https://proandroiddev.com/nothing-else-matters-in-kotlin-994a9ef106fc)
2. [Leonardo Aramaki's article](https://medium.com/@aramaki/how-kotlin-can-help-us-with-error-handling-4c2265c9b50)