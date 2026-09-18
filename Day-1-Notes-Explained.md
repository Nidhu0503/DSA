# Handwritten Teaching Notes Summary: Prime Numbers and Optimization

## Introduction / Agenda

Topics covered:

* Prime Numbers
* Quizzes
* Square Root
* LinkedIn influencer-level speeches in 2 months
* Next 2 months: topics and schedule

The class combines core algorithms, soft skills, and a longer learning plan.

## Checking Whether a Number Is Prime

### Definition

A number `N` is prime if it has exactly two factors:

1. `1`
2. `N`

`1` is not prime because it has only one factor.

### Naive Approach

```text
bool isPrime(N) {
    count = 0;

    for (i = 1 to N) {
        if (N % i == 0) {
            count++;
        }
    }

    return count == 2;
}
```

This checks every number from `1` to `N`, so its time complexity is:

```text
O(N)
```

### Why This Is Slow

Assumption:

```text
10^8 operations ≈ 1 second
```

Examples:

| N       | Approximate operations |                   Approximate time |
| ------- | ---------------------: | ---------------------------------: |
| `10^1`  |                   `10` |                            Instant |
| `10^8`  |                 `10^8` |                           1 second |
| `10^18` |                `10^18` | About 10^10 seconds, or 31.7 years |

The naive approach is impractical for large inputs.

## Factor Pair Observation

If:

```text
a × b = N
```

then both `a` and `b` are factors of `N`.

For example, factor pairs of `24`:

```text
1 × 24
2 × 12
3 × 8
4 × 6
```

After reaching the square root, factor pairs repeat in reverse order.

For `N = 24`:

```text
sqrt(24) ≈ 4.89
```

So checking factors only until `4` is enough.

Key observation:

```text
If N has a factor larger than sqrt(N),
it must have a matching factor smaller than sqrt(N).
```

Therefore, we only need to test divisors up to `sqrt(N)`.

## Optimized Prime Check

```text
bool isPrime(N) {
    if (N == 1) {
        return false;
    }

    for (i = 2; i * i <= N; i++) {
        if (N % i == 0) {
            return false;
        }
    }

    return true;
}
```

Time complexity:

```text
O(sqrt(N))
```

Using `i * i <= N` avoids explicitly calculating the square root.

For `N = 10^18`:

```text
sqrt(10^18) = 10^9
```

This is still large, but dramatically better than checking all `10^18` values.

## Gauss Sum: Sum of First N Natural Numbers

Problem:

```text
1 + 2 + 3 + ... + 100
```

Gauss pairs values from both ends:

```text
1 + 100 = 101
2 + 99  = 101
3 + 98  = 101
...
```

General derivation:

```text
S = 1 + 2 + ... + N
S = N + (N - 1) + ... + 1

2S = (N + 1) + (N + 1) + ... + (N + 1)

2S = N(N + 1)

S = N(N + 1) / 2
```

Formula:

```text
Sum of first N natural numbers = N(N + 1) / 2
```

This changes the solution from:

```text
O(N)
```

to:

```text
O(1)
```

The lesson is the same as prime-number optimization: look for a mathematical pattern before writing a large loop.

## Dividing N by 2 Repeatedly

Question:

```text
How many times can N be divided by 2 until it becomes 1?
```

Examples:

```text
2  -> 1                  = 1 time
4  -> 2 -> 1             = 2 times
8  -> 4 -> 2 -> 1        = 3 times
9  -> 4 -> 2 -> 1        = 3 times
15 -> 7 -> 3 -> 1        = 3 times
27 -> 13 -> 6 -> 3 -> 1  = 4 times
```

This is approximately:

```text
floor(log2(N))
```

Useful identity:

```text
log_a(a^n) = n
```

## Perfect Square / Integer Square Root

Problem:

Given `N`, find the largest integer `x` such that:

```text
x × x <= N
```

For example:

```text
N = 100
sqrt(100) = 10
```

For a non-perfect square:

```text
N = 27
integer sqrt(27) = 5
```

because:

```text
5 × 5 = 25
6 × 6 = 36
```

## Naive Integer Square Root

```text
for (i = 1 to N) {
    if (i * i == N) {
        return i;
    }
}
```

Time complexity:

```text
O(N)
```

This is slow for large values.

## Binary Search Approach for Square Root

Instead of checking every number, use binary search.

For `N = 100`:

```text
Search range: 1 to 100
Mid = 50, 50 × 50 is too large
Search range: 1 to 49

Mid = 24, 24 × 24 is too large
Search range becomes smaller

Continue until reaching 10
```

Each step removes about half of the remaining range:

```text
N -> N/2 -> N/4 -> N/8 -> ... -> 1
```

Time complexity:

```text
O(log N)
```

Example comparison for `N = 2^60`:

| Approach                |  Approximate work |
| ----------------------- | ----------------: |
| Check until square root | `2^30` operations |
| Binary search           |   `60` operations |

Binary search is much more efficient for large search spaces.

## Common Data Structure and Algorithm Applications

| Real-world feature              | Concept              |
| ------------------------------- | -------------------- |
| Undo / Redo                     | Stack                |
| Autocomplete                    | Trie                 |
| Facebook mutual friends         | BFS                  |
| Google Maps shortest path       | Dijkstra's Algorithm |
| Word processing / text problems | Dynamic Programming  |


Doubts - 

**How the √N insight is derived (step-by-step, exactly as taught)**

The notes build this insight carefully from concrete examples, not from a sudden formula. Here’s the exact logical path they follow.

### 1. Start with the basic definition of factors
If a number `a` divides `N` (i.e. `N % a == 0`), then there must exist another integer `b` such that:

\[
a \times b = N \quad \Rightarrow \quad b = \frac{N}{a}
\]

So **every factor comes in a pair** `(a, b)`.

### 2. Look at real examples (the tables on page 3)

**Example 1 – N = 24**

| i | N/i |
|---|-----|
| 1 | 24 |
| 2 | 12 |
| 3 | 8  |
| 4 | 6  |
| 6 | 4  |
| 8 | 3  |
| 12| 2  |
| 24| 1  |

Notice two clear parts:
- **Part I** (i ≤ √24 ≈ 4.9): 1, 2, 3, 4
- **Part II** (i > √24): 6, 8, 12, 24

The pairs simply reverse:  
(1,24) ↔ (24,1)  
(2,12) ↔ (12,2)  
(3,8) ↔ (8,3)  
(4,6) ↔ (6,4)

**Example 2 – N = 100**

| i  | N/i |
|----|-----|
| 1  | 100 |
| 2  | 50  |
| 4  | 25  |
| 5  | 20  |
| 10 | 10  |  ← exactly √100
| 20 | 5   |
| 25 | 4   |
| 50 | 2   |
| 100| 1   |

Again:
- Everything up to √100 = 10 appears in Part I.
- After that the pairs just flip.

### 3. The general observation (the “crucial insight”)

From the tables the teacher writes three observations:

1. If we only iterate in **Part I**, we already get **all** the factors of N.  
   (Because every factor in Part II is just the pair of something already found in Part I.)

2. In Part I the smallest i is always **1**.

3. In Part I the **largest** i we ever need is **√N**.

Why √N is the boundary?
- When `i < √N`, then `N/i > √N`.
- When `i = √N` (if N is a perfect square), then `i = N/i`.
- When `i > √N`, then `N/i < √N` → this is just the reverse of a pair we already saw.

Therefore the moment `i` crosses √N, we start seeing only the **mirrored** factors.

### 4. Consequence for the isPrime algorithm

To decide whether N is prime we only care whether it has **any** factor other than 1 and itself.

- We already know 1 is always a factor.
- If we find **any** factor in the range `2 … √N`, then N is composite.
- If we find **none**, then the only remaining possible factor is N itself → N is prime.

That is why the loop becomes:

```text
for (i = 2; i * i <= N; i++) {   // stop at √N
    if (N % i == 0) return false;
}
return true;   // no factors found → prime
```

### Visual summary of the derivation

```
Factors always come in pairs (a, N/a)

        ← Part I →          ← Part II →
1 …………………… √N …………………… N

Everything after √N is just the reverse of something before √N.
So checking only up to √N is enough to discover every factor.
```

This is pure observation from the factor tables + the pairing property. No advanced number theory is required — just looking carefully at how the pairs behave.



**Perfect Square / Integer Square Root Problem (Pages 7–9)**

### Problem Statement
Given a positive integer `N`, find the **integer square root** of `N`.  
That means: return the **largest integer `x` such that `x * x ≤ N`**.

Examples from the notes:
- `N = 100` → `10` (because `10*10 = 100`)
- `N = 25` → `5`
- `N = 20` → `4` (because `4*4 = 16 ≤ 20`, but `5*5 = 25 > 20`)
- `N = 64` → `8`

You can also use the same logic to check if `N` is a **perfect square** (i.e., if `x * x == N`).

---

### 1. Naive Approach (Loop from 1 to N)

```cpp
int sqrtNaive(int N) {
    for (int i = 1; i <= N; i++) {
        if (i * i == N) {
            return i;               // perfect square
        }
        if (i * i > N) {
            return i - 1;           // largest integer whose square ≤ N
        }
    }
    return 0; // only for N = 0
}
```

**Why it is bad**  
- It does up to `N` iterations.  
- For large `N` (e.g. `10^18`) this is extremely slow (same problem we saw with the naive prime checker).

---

### 2. Slightly Better Ranges (still not great)

The notes discuss three possible upper limits:

| Option       | Loop range          | Why it works / why it is still weak                  |
|--------------|---------------------|-----------------------------------------------------|
| a) `N`       | `1 → N`             | Correct but too slow                                |
| b) `N/2`     | `1 → N/2`           | Still linear, only halves the work                  |
| c) `√N`      | `1 → √N`            | Ideal, but **circular** — you don’t know `√N` yet   |
| d) `log N`   | —                   | This is the direction we will go with binary search |

---

### 3. Binary Search Approach (the key idea on pages 8–9)

Instead of checking every number, we **search** for the answer in the range `[1 … N]` using binary search.

#### Core Idea
We maintain a search range `[low, high]`.  
At every step we look at the middle value `mid` and decide:

- If `mid * mid == N` → we found the exact square root.
- If `mid * mid < N` → the answer is at least `mid`, so search the right half.
- If `mid * mid > N` → the answer is smaller than `mid`, so search the left half.

Because the range is halved every time, the number of steps is only about `log₂ N`.

#### Detailed Dry-run for N = 100 (exactly as shown on page 8)

```
Initial range: [1, 100]

mid = 50   → 50*50 = 2500 > 100  → too big → new range [1, 49]
mid = 25   → 25*25 = 625  > 100  → too big → new range [1, 24]
mid = 12   → 12*12 = 144  > 100  → too big → new range [1, 11]
mid = 6    → 6*6   = 36   < 100  → too small → new range [7, 11]
mid = 9    → 9*9   = 81   < 100  → too small → new range [10, 11]
mid = 10   → 10*10 = 100 == 100  → found!
```

You can also see the successive halving written in the notes:

```
N → N/2 → N/4 → N/8 → … → 1
```

This is the classic binary-search reduction → **O(log N)** time.

---

### 4. Clean C++ Implementation (Binary Search)

```cpp
#include <iostream>
using namespace std;

long long integerSqrt(long long N) {
    if (N == 0 || N == 1) return N;

    long long low = 1;
    long long high = N;
    long long ans = 1;               // will store the floor(sqrt(N))

    while (low <= high) {
        long long mid = low + (high - low) / 2;   // safer than (low+high)/2

        // To avoid overflow we compare mid with N/mid instead of mid*mid
        if (mid <= N / mid) {
            // mid*mid <= N
            ans = mid;               // mid is a possible answer
            low = mid + 1;           // try to find a bigger one
        } else {
            // mid*mid > N
            high = mid - 1;          // go left
        }
    }
    return ans;
}

// Optional: check if perfect square
bool isPerfectSquare(long long N) {
    long long root = integerSqrt(N);
    return root * root == N;
}

int main() {
    cout << integerSqrt(100) << endl;   // 10
    cout << integerSqrt(20)  << endl;   // 4
    cout << integerSqrt(25)  << endl;   // 5
    cout << integerSqrt(1)   << endl;   // 1
    cout << integerSqrt(0)   << endl;   // 0

    cout << boolalpha;
    cout << isPerfectSquare(100) << endl; // true
    cout << isPerfectSquare(20)  << endl; // false
}
```

#### Important implementation notes
- We use `long long` because `N` can be up to `10^18` in many problems.
- Instead of writing `mid * mid <= N` (which can overflow), we write `mid <= N / mid`. This is safer.
- We keep updating `ans` whenever we find a valid `mid` that satisfies `mid*mid ≤ N`. At the end `ans` holds the largest such value.

---

### 5. Why Binary Search is dramatically better (Page 9)

The notes compare two approaches for a huge number `N = 2^60`:

| Method              | Approximate steps          | Practical?      |
|---------------------|----------------------------|-----------------|
| Loop up to √N       | √(2⁶⁰) ≈ 2³⁰ ≈ 1 billion   | Still slow      |
| Binary Search       | log₂(2⁶⁰) = 60             | Extremely fast  |

This is the same philosophy we saw with primes:  
**reduce the search space using mathematical observations** (here the monotonicity of the square function).

---

### Summary of the teaching flow on pages 7–9

1. Show the naïve linear loop → too slow.  
2. Discuss possible better upper bounds (N/2, √N) and why √N is circular.  
3. Introduce the binary-search idea with a concrete walk-through on N = 100.  
4. Highlight that the range shrinks as N → N/2 → N/4 → … → 1 (log N steps).  
5. Compare √N vs log N on a very large number to drive the point home.

Would you like me to also show the recursive version of the binary search, or a version that works for very large N using floating-point + correction, or more dry-runs?
Hello
