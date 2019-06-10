Add your answers to the Algorithms exercises here.

# Exercise I

```
a)  a = 0                                       # O(1)
    while (a < n * n * n):                      # O(n^3)
      a = a + n * n                             # O(1)
```

Overall complexity as it is: O(1) + O(n^3) \* O(1) => **O(n^3)**. Still, at most
cases in the default (binary) arithmetic, the line `a = a + n * n` allows to
reduce the complexity to the `linear` because for the given value of `n` this
while loop is going to execute `n` times. The final time complexity is **O(n)**.

```
b)  sum = 0                                     # O(1)
    for i in range(n):                          # O(n)
      i += 1                                    # O(1)
      for j in range(i + 1, n):                 # O(n)
        j += 1                                  # O(1)
        for k in range(j + 1, n):               # O(n)
          k += 1                                # O(1)
          for l in range(k + 1, 10 + k):        # O(1)
            l += 1                              # O(1)
            sum += 1                            # O(1)
```

Overall complexity: O(1) + O(n) _ ( O(1)+O(n) _ ( O(1)_O(n) _ ( O(1) + O(1) \*
(O(1) + O(1) )) ) ) => **O(n^3)**.

```
c)  def bunnyEars(bunnies):                     # header, does not count
      if bunnies == 0:                          # O(1)
        return 0                                # O(1)

      return 2 + bunnyEars(bunnies-1)           # O(n)
```

Overall complexity: O(1) + O(n) = **O(n)**. Requires some (linear) additional
space on the call stack for the recursive calls. The `n` is the size of input,
so there the number of `bunnies`.

# Exercise II

Suppose that you have an _n_-story building and plenty of eggs. Suppose also
that an egg gets broken if it is thrown off floor _f_ or higher, and doesn't get
broken if dropped off a floor less than floor _f_. Devise a strategy to
determine the value of _f_ such that the number of dropped eggs is minimized.

Write out your proposed algorithm in plain English or pseudocode and give the
runtime complexity of your solution.

---

As long as we have multiple eggs and a finite number of floors in the building
(ok, it can work with the infinite number of floors as well after small
changes), we can use a **divide and conquer** strategy to determine the maximum
floor we can throw the egg from. By default it makes our time complexity O(log
n) because we cut down half of the search set at each step.

Actually, we cannot be 100% sure whether the floor _f_ exists in the building,
so we can use two versions of binary search that do **upper bound** and **lower
bound** respectively. Their difference is going to tell us whether the floor _f_
exists in the building - when it's 0, then the floor _f_ does not exist there,
but when the difference is equal to 1, then we can be sure that the floor exists
and what is the actual level of it.

Still, I think that the solution may be too complex for the actual requirements
of the challenge, so the easiest way is to use the plain **lower bound**.

1. Start in the middle level of the building (n/2, where n is the length of the
   sorted array of levels). Assign the left and right bounds to `L` and `R`
   respectively. (L = 0, R = n-1)
2. While the value of `L` is smaller than `R`, repeat following steps:
3. Select the middle level between `L` and `R`. `m = math.floor((L+R)/2)`
4. Check whether the egg drops when you throw it from the level `m`.
5. If it doesn't, then do the binary search between `L = m+1` and `R`.
   Otherwise, do the binary search between `L` and `R = m`.
6. When `L` is no longer strictly smaller than `R`, return `L`. `L` is the
   lowest floor we can be sure that the egg will crash if we drop it from.

---

Another approach: **binsearch on a result**. Especially useful when we have an
infinite number of floors. Also **O(log n)**.

1. We start with the floors `L = 0` and `R = 1` and check the result of dropping
   an egg from the floor `R`.
2. If the egg doesn't crash, then make the value of `R` the double of what was
   it in the previous step, then repeat the drop test.
3. If the egg crashes, dropped from the given floor, then do `binsearch`, or
   more precisely, the `lower bound` on L and R, as described above.
4. The algorithm returns the number of the lowest floor we can be sure that the
   egg will crash if we drop it from.
