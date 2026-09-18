# Day 2.1 — Number Systems, Binary Representation, Signed Numbers, and Bitwise Operators

This chapter covers number systems, binary arithmetic, signed integer representation, integer ranges, bitwise operators, and a classic XOR problem.

> **Note:** In the ternary example, `(0210)₃ = 21₁₀`, not `64₁₀`. The expansion that equals `64` is `(2101)₃`.

---

## 1. Number Systems

A number system has a **base** (or radix). Each digit position represents a power of that base.

### Decimal (Base 10)

* Digits: `0` through `9`
* Place values: powers of `10`

$$
734 = 7 \times 10^2 + 3 \times 10^1 + 4 \times 10^0
$$

For an `N`-digit decimal number, place values range from \(10^{N-1}\) to \(10^0\).

### Octal (Base 8)

* Digits: `0` through `7`
* Place values: powers of `8`

$$
(132)_8 = 1 \times 8^2 + 3 \times 8^1 + 2 \times 8^0
$$

$$
= 64 + 24 + 2 = 90
$$

Another example:

$$
(125)_8 = 1 \times 8^2 + 2 \times 8^1 + 5 \times 8^0
$$

$$
= 64 + 16 + 5 = 85
$$

### Ternary (Base 3)

* Digits: `0`, `1`, and `2`
* Place values: powers of `3`

$$
(2101)_3 = 2 \times 3^3 + 1 \times 3^2 + 0 \times 3^1 + 1 \times 3^0
$$

$$
= 54 + 9 + 0 + 1 = 64
$$

### Binary (Base 2)

* Digits: `0` and `1`
* Place values: powers of `2`

$$
(10110)_2 = 1 \times 2^4 + 0 \times 2^3 + 1 \times 2^2 + 1 \times 2^1 + 0 \times 2^0
$$

$$
= 16 + 0 + 4 + 2 + 0 = 22
$$

---

## 2. Decimal to Binary Conversion

To convert a decimal number to binary:

1. Repeatedly divide the number by `2`.
2. Record each remainder.
3. Read the remainders from bottom to top.

| Decimal |         Binary |
| ------: | -------------: |
|  \(28\) |  \((11100)_2\) |
|  \(35\) | \((100011)_2\) |
|  \(19\) |  \((10011)_2\) |
|  \(25\) |  \((11001)_2\) |

Example for \(28\):

```text
28 ÷ 2 = 14 remainder 0
14 ÷ 2 = 7  remainder 0
7  ÷ 2 = 3  remainder 1
3  ÷ 2 = 1  remainder 1
1  ÷ 2 = 0  remainder 1

28₁₀ = 11100₂
```

---

## 3. Binary Addition

Decimal addition creates a carry when a digit sum is `10` or greater. Binary addition creates a carry when a bit sum is `2` or greater.

| Addition    | Result         |
| ----------- | -------------- |
| `0 + 0`     | `0`            |
| `0 + 1`     | `1`            |
| `1 + 0`     | `1`            |
| `1 + 1`     | `0`, carry `1` |
| `1 + 1 + 1` | `1`, carry `1` |

Example:

```text
  0110
+ 1010
------
 10000
```

$$
(0110)_2 = 6,\qquad (1010)_2 = 10,\qquad (10000)_2 = 16
$$

---

## 4. Binary Terminology and Ranges

* A bit is **set** when its value is `1`.
* A bit is **unset** or **cleared** when its value is `0`.
* **MSB**: Most Significant Bit — the leftmost bit.
* **LSB**: Least Significant Bit — the rightmost bit.

### Interval notation

| Interval     | Number of values |
| ------------ | ---------------: |
| \([1, N]\)   |            \(N\) |
| \([0, N-1]\) |            \(N\) |
| \([a, b]\)   |        \(b-a+1\) |

### Sum of powers of 2

$$
2^0 + 2^1 + 2^2 + \dots + 2^{N-1} = 2^N - 1
$$

This identity explains why an `N`-bit **unsigned** number can represent values from:

$$
0 \text{ to } 2^N - 1
$$

---

## 5. Signed Numbers and Two's Complement

Computers normally represent signed integers using **two's complement**.

For an `N`-bit signed integer:

* The MSB has weight \(-2^{N-1}\).
* Every other bit keeps its usual positive power-of-two weight.

### Example: 8-bit representation of `+10`

```text
00001010
```

$$
10 = 8 + 2
$$

### Example: 8-bit representation of `-10`

```text
11110110
```

$$
-10 = -2^7 + 2^6 + 2^5 + 2^4 + 2^2 + 2^1
$$

$$
= -128 + 64 + 32 + 16 + 4 + 2 = -10
$$

### Finding the negative representation

To represent `-x` in two's complement:

1. Write `x` in binary.
2. Flip every bit.
3. Add `1`.

```text
+10 = 00001010
Flip = 11110101
Add 1 = 11110110  → -10
```

### Range of an N-bit signed integer

$$
[-2^{N-1},\ 2^{N-1}-1]
$$

|  Bits |      Minimum |       Maximum |
| ----: | -----------: | ------------: |
|     2 |       \(-2\) |         \(1\) |
|     3 |       \(-4\) |         \(3\) |
|     4 |       \(-8\) |         \(7\) |
|     5 |      \(-16\) |        \(15\) |
| \(N\) | \(-2^{N-1}\) | \(2^{N-1}-1\) |

The range is asymmetric because zero has no separate positive/negative representation, leaving one additional negative value.

---

## 6. Common Integer Ranges

> Exact type sizes can vary by language and platform. The following are the conventional sizes used by Java and most modern systems.

| Type    | Bits |                   Range |
| ------- | ---: | ----------------------: |
| `byte`  |    8 |         \([-128, 127]\) |
| `short` |   16 |     \([-32768, 32767]\) |
| `int`   |   32 | \([-2^{31}, 2^{31}-1]\) |
| `long`  |   64 | \([-2^{63}, 2^{63}-1]\) |

Useful approximations:

$$
2^{10} \approx 10^3
$$

$$
2^{20} \approx 10^6
$$

$$
2^{30} \approx 10^9
$$

$$
2^{60} \approx 10^{18}
$$

---

## 7. Bitwise Operators

Bitwise operators work independently on each bit.

| Operator | Name        | Meaning                          |
| -------- | ----------- | -------------------------------- |
| `&`      | AND         | `1` only when both bits are `1`  |
| `\|`     | OR          | `1` when at least one bit is `1` |
| `^`      | XOR         | `1` when the bits differ         |
| `~`      | NOT         | Flips every bit                  |
| `<<`     | Left shift  | Shifts bits left                 |
| `>>`     | Right shift | Shifts bits right                |

### Truth table

| `a` | `b` | `a & b` | `a \| b` | `a ^ b` |
| --: | --: | ------: | -------: | ------: |
|   0 |   0 |       0 |        0 |       0 |
|   0 |   1 |       0 |        1 |       1 |
|   1 |   0 |       0 |        1 |       1 |
|   1 |   1 |       1 |        1 |       0 |

For NOT:

| `a` | `~a` |
| --: | ---: |
|   0 |    1 |
|   1 |    0 |

### Example

```text
a = 29 = 00011101
b = 18 = 00010010
```

```text
a & b = 00010000 = 16
a | b = 00011111 = 31
a ^ b = 00001111 = 15
```

`~a` flips all bits. For a signed 32-bit `int`, `~29` is `-30`; the result depends on the integer width used.

### Even or odd check

The LSB determines whether an integer is even or odd.

```cpp
if ((a & 1) == 0) {
    // a is even
} else {
    // a is odd
}
```

* LSB is `0` → even
* LSB is `1` → odd

### Important properties

For `&`, `|`, and `^`:

```text
a ऑप b = b ऑप a                  // Commutative
(a ऑप b) op c = a op (b op c)    // Associative
```

Useful identities:

```text
a ^ a = 0
a ^ 0 = a

a & a = a
a | a = a
```

> `&`, `|`, and `^` are bitwise operators.
> `&&` and `||` are logical operators: they evaluate whole expressions as true/false and use short-circuit evaluation.

---

## 8. Problem: Find the Unique Element

### Problem statement

Given an array in which every element appears exactly twice except for one element, find the unique element.

### Key idea

XOR has two useful properties:

```text
x ^ x = 0
x ^ 0 = x
```

Since XOR is commutative and associative, every duplicate pair cancels out. The only remaining value is the unique element.

### C++ solution

```cpp
#include <iostream>
#include <vector>

using namespace std;

int findUnique(const vector<int>& arr) {
    int ans = 0;

    for (int x : arr) {
        ans ^= x;
    }

    return ans;
}

int main() {
    vector<int> arr1 = {4, 7, 6, 4, 8, 7, 6};
    cout << findUnique(arr1) << endl; // 8

    vector<int> arr2 = {1, 4, 4, 2, 1};
    cout << findUnique(arr2) << endl; // 2
}
```

### Complexity

| Metric      | Complexity |
| ----------- | ---------: |
| Time        |   \(O(N)\) |
| Extra space |   \(O(1)\) |

This approach is preferred because it is concise, uses constant extra space, and avoids auxiliary data structures. It is not generally important to choose it because XOR is “faster” than arithmetic; the stronger reason is that it directly uses the duplicate-pair constraint.

---

## 9. Quick Reference

```text
N-bit unsigned range:  0 to 2^N - 1
N-bit signed range:   -2^(N-1) to 2^(N-1) - 1

MSB: leftmost bit
LSB: rightmost bit

Even: (x & 1) == 0
Odd:  (x & 1) == 1

Duplicate cancellation:
x ^ x = 0
```

The core progression is:

```text
Number systems
→ Binary representation
→ Binary arithmetic
→ Two's complement signed integers
→ Bitwise operators
→ XOR-based problem solving
```
