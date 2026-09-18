# Day 3: Bit Manipulation

## 1. Negative Numbers and Two’s Complement

Computers store negative integers using two’s complement.

```text
-a = ~a + 1
```

Here, `~a` means bitwise NOT: every bit is flipped.

Example using 8 bits:

```text
 10 = 00001010
~10 = 11110101
~10 + 1 = 11110110 = -10
```

Verification:

```text
11110110
= -128 + 64 + 32 + 16 + 4 + 2
= -10
```

To find the binary representation of a negative number:

```text
1. Write the positive number in binary.
2. Flip all bits.
3. Add 1.
```

Examples:

```text
 23 → -23
 30 → -30
```

For any positive number `n`:

```text
~n = -1 - n
-n = ~n + 1
```

Two’s complement is useful because positive and negative numbers can be added using the same binary addition hardware.

---

## 2. Left Shift and Right Shift

### Left Shift

`a << k` moves all bits of `a` by `k` positions to the left. The empty positions on the right are filled with `0`.

```text
a << k = a × 2^k
```

This works only when the result stays within the integer range.

Example: `a = 10`

```text
10 = 00001010

10 << 1 = 00010100 = 20
10 << 2 = 00101000 = 40
10 << 3 = 01010000 = 80
10 << 4 = 10100000 = -96 in signed 8-bit representation
```

In signed 8-bit numbers, the range is:

```text
-128 to 127
```

So `10 << 4` overflows the signed 8-bit range.

### Right Shift

`a >> k` moves all bits of `a` by `k` positions to the right.

For positive numbers:

```text
a >> k = a / 2^k
```

Example:

```text
40 >> 1 = 20
40 >> 2 = 10
40 >> 3 = 5
40 >> 4 = 2
```

For negative signed integers, right-shift behavior should not be relied upon in C++. Use unsigned integers when a logical right shift is required.

---

## 3. Check Whether the i-th Bit Is Set

Given a number `N` and a position `i`, determine whether the bit at position `i` is `1`.

Bit positions are counted from the right, starting at `0`.

Example:

```text
21 = 10101

Position: 4 3 2 1 0
Bit:      1 0 1 0 1
```

### Method 1: Right Shift

```cpp
bool checkBit(int N, int i) {
    return ((N >> i) & 1) == 1;
}
```

### Method 2: Bit Mask

```cpp
bool checkBit(int N, int i) {
    return (N & (1 << i)) != 0;
}
```

Examples:

```text
N = 21 = 10101, i = 2 → Set
N = 25 = 11001, i = 2 → Unset
N = 106 = 1101010, i = 2 → Unset
N = 106 = 1101010, i = 3 → Set
```

---

## 4. Count Set Bits

A set bit is a bit with value `1`.

Example:

```text
25 = 11001 → 3 set bits
40 = 101000 → 2 set bits
35 = 100011 → 3 set bits
```

### Method 1: Check All 32 Bits

```cpp
int countSetBits(int N) {
    int count = 0;

    for (int i = 0; i < 32; i++) {
        if ((N & (1 << i)) != 0) {
            count++;
        }
    }

    return count;
}
```

### Method 2: Repeated Right Shift

```cpp
int countSetBits(int N) {
    int count = 0;

    while (N > 0) {
        if (N & 1) {
            count++;
        }

        N = N >> 1;
    }

    return count;
}
```

The second method runs approximately `log₂(N)` times.

---

## 5. Find the Least Significant Set Bit

The least significant set bit is the rightmost bit with value `1`.

Example:

```text
44 = 101100
```

The rightmost `1` is at position `2`.

```cpp
int leastSignificantSetBit(int N) {
    for (int i = 0; i < 32; i++) {
        if ((N & (1 << i)) != 0) {
            return i;
        }
    }

    return -1;
}
```

If `N` is `0`, there is no set bit, so the function returns `-1`.

---

## 6. Find Two Unique Elements

Problem:

Given an array where every number appears exactly twice except for two unique numbers, find both unique numbers.

Example:

```text
{3, 4, 5, 4, 7, 5}

Answer: 3 and 7
```

Another example:

```text
{4, 9, 1, 8}

Answer: 4 and 8
```

### XOR Properties

```text
a ^ a = 0
a ^ 0 = a
a ^ b = 0 only when a = b
```

### Approach

```text
1. XOR every array element.
   The result becomes A ^ B, where A and B are the two unique numbers.

2. Find any set bit in A ^ B.
   This bit is different in A and B.

3. Divide the array into two groups:
   - Numbers with that bit set
   - Numbers with that bit unset

4. XOR each group separately.
   Duplicate values cancel out, leaving A and B.
```

### C++ Code

```cpp
#include <iostream>
#include <vector>

using namespace std;

bool checkBit(int num, int pos) {
    return (num & (1 << pos)) != 0;
}

pair<int, int> findTwoUnique(const vector<int>& arr) {
    int xorAll = 0;

    for (int x : arr) {
        xorAll ^= x;
    }

    int position = 0;

    for (int i = 0; i < 32; i++) {
        if (checkBit(xorAll, i)) {
            position = i;
            break;
        }
    }

    int setGroup = 0;
    int unsetGroup = 0;

    for (int x : arr) {
        if (checkBit(x, position)) {
            setGroup ^= x;
        } else {
            unsetGroup ^= x;
        }
    }

    return {setGroup, unsetGroup};
}

int main() {
    vector<int> arr1 = {3, 4, 5, 4, 7, 5};

    auto [a, b] = findTwoUnique(arr1);
    cout << a << " " << b << endl;

    return 0;
}
```

Time complexity:

```text
O(N)
```

Extra space:

```text
O(1)
```

## Quick Revision

```text
-a = ~a + 1

a << k = a × 2^k
a >> k = a / 2^k for positive numbers

Check i-th bit:
(N & (1 << i)) != 0

Count set bits:
Keep checking N & 1, then right shift N

Least significant set bit:
Find the first set bit from the right

Two unique numbers:
XOR all values
Find a differing bit
Split into two groups
XOR each group
```
