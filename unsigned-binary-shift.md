# Unsigned right shift

Today I came across this piece of code in [React's scheduler](https://github.com/facebook/react/blob/master/packages/scheduler/src/SchedulerMinHeap.js#L59):

```js
const halfLength = length >>> 1;
```

What looks like a git merge gone wrong, is actually an operator called [Unsigned right shift](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Unsigned_right_shift).
It shifts a given number by n bits to the right. But why would you want to shift `length` by 1 bit, and why would you call the result `halfLength`?

Digging into it a little bit more I found an [wikipedia article about the Logical shift](https://en.wikipedia.org/wiki/Logical_shift#:~:text=Shifting%20right%20by%20n%20bits), which states that:

> Shifting right by n bits on an unsigned binary number has the effect of dividing it by 2n (rounding towards 0).

Example:

```js
8 >>> 0; // 8
8 >>> 1; // 4
8 >>> 2; // 2
8 >>> 3; // 1
```

Now, the intent of using the operator is clear. Using unsigned right shift on length by 1, is the same as dividing `length` by 2.

```js
const halfLength = Math.floor(length / 2);
```

You can even see a commit that [replaces a similar code](https://github.com/facebook/react/commit/7309c5f93469266729578f1d0e0d273604b45da4#diff-56e2daa600b00c7aeb1895db77b69de74aa873ccf3afa481d3bb0e9b62e1caa3) with the unsigned right shift operator.
