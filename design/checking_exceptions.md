# checking exceptions

exceptions are expensive and should only be used in *exceptional* situations
If we know there's a possiblity of failure in our code which can easily be detected, we can mitigate it by __checking__ instead of wrapping the code in a __try-catch__

We already do this normally:
Take our ``for`` loop for example. When iterating through, it's possible for us to raise an ``OutOfBounds`` exception if we try to access an index outside the list.

```Kotlin
val list = listOf(1, 2, 3)

// throws an ``OutOfBounds`` exception trying to access index outside of list
print(list[3])

// doesn't throw an exception
for (value in list):
    print(value)
```

it's obvious that the functions perform very different tasks, but they work well enough in this example to show that effectiveness of prevention.

IMO this is only an effective measure for *your* code and you still need some way to indicate an checked operation has not succeeded. In this case, if we opt to fail without throwing an exception, we lose access to our stacktrace which can in itself be a terrible thing. Moreover, we don't really have the option of checking when using external libraries.

## Sources

- [cgi](https://web.archive.org/web/20140430045330/http://c2.com/cgi-bin/wiki?CheckDontCatch)