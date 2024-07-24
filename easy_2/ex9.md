# My Solution

It prints:

```
BOB, BOB
```

Both variables `name` and `save_name` point to the same string object `'Bob'` in memory.

In the first code snippet, `name` gets re-assigned and points to another string object in memory; this means `name` and ``save_name` are each pointing to different string objects in memory and thus hold different values.

In the second code snippet, the string object pointed to by `name` is changed or mutated in-place to `BOB` by the `#upcase!` method call. Since both `name` and `save_name` variables point to the same changed string object, `save_name`'s value is also `BOB`.
