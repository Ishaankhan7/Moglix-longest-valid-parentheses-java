# Longest Valid Parentheses

A classic (and honestly kind of tricky) string problem: given a string made up of only `(` and `)`, find the length of the longest substring that's a valid, well-formed sequence of parentheses.

I picked this one up while practicing for interviews, and ended up solving it four different ways — partly because the "optimal" solution isn't obvious the first time you see it, and partly because I wanted to actually understand *why* each approach works instead of just memorizing it.

## Problem

Given a string `s` containing just `'('` and `')'`, return the length of the longest valid parentheses substring.

**Examples**

```
Input:  "(()"
Output: 2          // "()"

Input:  ")()())"
Output: 4          // "()()"

Input:  ""
Output: 0
```

**Constraints**
- `0 <= s.length <= 3 * 10^4`
- `s[i]` is either `(` or `)`

## What's in this repo

I wrote out every approach I could think of, roughly in the order I discovered them while working through the problem:

| File | Approach | Time | Space |
|---|---|---|---|
| `BruteForce.java` | Check every substring for validity | O(n³) | O(n) |
| `DPSolution.java` | Dynamic programming | O(n) | O(n) |
| `StackSolution.java` | Stack of indices | O(n) | O(n) |
| `TwoPassSolution.java` | Left-to-right + right-to-left counters | O(n) | O(1) |

### Brute force
The straightforward way — try every possible substring, check if it's valid, and keep track of the longest one that is. It works, but it's O(n³), so it'll time out on anything but small inputs. Good for building intuition before jumping to the faster approaches.

### Dynamic programming
`dp[i]` holds the length of the longest valid substring *ending* at index `i`. The tricky part is handling the case where a valid substring is immediately preceded by another valid substring — you have to reach back and stitch the two together. Took me a couple of tries on paper to get the index math right.

### Stack
This one clicked the fastest for me. Push `-1` onto the stack as a base marker, then push indices of `(` characters. When you hit a `)`, pop the stack — if it's now empty, push the current index as the new base; otherwise, the distance between the current index and whatever's on top of the stack is a valid length.

### Two-pass counter
The most elegant, but also the one I had to sit with the longest to actually believe. Scan left to right counting `(` and `)`; when they're equal you've got a valid stretch, and if `)` ever outnumbers `(` you reset. That alone misses cases like `"(()"` where `(` never gets "cancelled out," so you do a second pass right to left to catch those. O(1) space, no extra data structures.

## Running it

Each solution is a standalone class with its own method — no external dependencies, just plain Java. Compile and run whichever one you want to try:

```bash
javac StackSolution.java
java StackSolution
```

(Add your own `main` method with test cases, or wire these into a test runner of your choice.)

## Why bother with four solutions to one problem?

Mostly because the first two approaches that come to mind (brute force, then DP) aren't the ones you'd actually use in a real system, and I wanted to work my way up to the ones that are. Also, comparing them side by side made the trade-offs a lot more obvious than just reading about Big-O in the abstract.

Feel free to open an issue if you spot a bug or have a cleaner way to write any of these — always happy to compare notes.
