## Don't Use the Assert Keyword

### Summary

Do not use the assert keyword.

### Details

Assertions written with the assert keyword are only enabled when a JVM flag is set. This is not what you want.

For production code, instead use libraries such as Guava's preconditions that are guaranteed to trigger.

If you have the assert keyword in your test code ask yourself how it is that you never noticed that your tests could not fail. After your period of self-reflection, replace the asserts with calls to an assertion library such as AssertJ.
