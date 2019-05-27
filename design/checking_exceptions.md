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

I think it's important to really define *when* we should be using exceptions. They should only be used under *exceptional* circumstances, which is to say

> **Exceptions should only be used for situations out of our program's control**

In my experience, the bulk of these situations such as network requests and file operations are IO bound. An IO bound operatin can fail or succeed based on external factors such as network connectivity, which is outside of the scope of our program.

I've recently started (rather naively) fleshing out the exception handling for my a project I've been working on for a while. I use two main external libraries for my repositories: ``Volley`` and ``SimpleJson`.

Since the implementation is hidden behind abstractions, I initially began with catching the exceptions thrown by both libraries and transforming them into exceptions my program knows about. After spending a bit more time studying exception handling, I'm not entirely sure if catching and transforming exceptions for ``SimpleJson`` is the correct move

Here's why:
``SimpleJson`` throws various exceptions if we try to parse something (like our server response in this case) that isn't properly formed. This is a scenario where exception checking is best utilized because we can now check the server response at each stage we parse it. But how do we fail or indicate failure gracefully? There are two possible solutions at this point:

1. Throw exception like the original solution
2. Monadic error handling

## comparison of the two

1. Throw exception like the original solution:
If we find that the input would generate an exception at any check point in our method, we can create a new ``RelicParseException`` with specific message about the point of failure and throw it. But that's not too much better than what we had originally; we would still have to catch the failure when it gets bubbled up.

2. Monadic error handling:
If we encapsulate the result of an operation in monadic result type, we can take advantage of Kotlin's explicit lack of enforced exception checks. This begins looking kind of like FP, but I'm not entirely averse to that.

    > *Moreover, it still allows us to handle failure we can control while still allowing unhandled exceptions to bubble further upstream*

This result type isn't meant to replace exceptions. It's meant for failures that are **easy to anticipate**, and preserves our ability to handle exceptions later without the verbosity of propogating ```try-catches``` through our function calls

It'll probably help if I add a more concrete example, but that'll wait until a bit later when I can also organize the thoughts in this entry.

## Sources

- [cgi](https://web.archive.org/web/20140430045330/http://c2.com/cgi-bin/wiki?CheckDontCatch)
- [you're better off using exceptions](https://eiriktsarpalis.wordpress.com/2017/02/19/youre-better-off-using-exceptions/)