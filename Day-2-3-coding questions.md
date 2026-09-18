# Bitwise Problems

## 1. Find the Unique Element

Problem: Given an array of N integers where every element appears exactly twice except one unique element, find that unique element.

Example:

```text
Input:  {4, 7, 6, 4, 8, 7, 6}
Output: 8
```

```cpp
#include <bits/stdc++.h>
using namespace std;

int findUnique(const vector<int>& arr) {
    int ans = 0;

    for (int value : arr) {
        ans ^= value;
    }

    return ans;
}

int main() {
    vector<int> arr1 = {4, 7, 6, 4, 8, 7, 6};
    cout << findUnique(arr1) << endl;

    vector<int> arr2 = {1, 4, 4, 2, 1};
    cout << findUnique(arr2) << endl;

    return 0;
}
```

## 2. Check Whether a Bit Is Set

Problem: Given `N` and bit position `i`, return true if the i-th bit of `N` is `1`.

```cpp
#include <bits/stdc++.h>
using namespace std;

bool checkBit(int num, int position) {
    return (num & (1 << position)) != 0;
}

int main() {
    int n1 = 8;

    if (checkBit(n1, 3)) {
        cout << "Yes, the bit is set" << endl;
    } else {
        cout << "No, the bit is not set" << endl;
    }

    int n2 = 4;

    if (checkBit(n2, 2)) {
        cout << "Yes, the bit is set" << endl;
    } else {
        cout << "No, the bit is not set" << endl;
    }

    return 0;
}
```

## 3. Count Set Bits

Problem: Given `N`, count how many bits have value `1`.

Example:

```text
35 = 100011

Answer: 3
```

### Approach 1: Check Every Bit

```cpp
#include <bits/stdc++.h>
using namespace std;

bool checkBit(int num, int position) {
    return (num & (1 << position)) != 0;
}

int countSetBits(int num) {
    int count = 0;

    for (int i = 0; i < 32; i++) {
        if (checkBit(num, i)) {
            count++;
        }
    }

    return count;
}

int main() {
    int n = 35;
    cout << countSetBits(n) << endl;

    return 0;
}
```

Time Complexity: `O(32)`
Space Complexity: `O(1)`

### Approach 2: Right Shift

This method runs approximately `log₂(N)` times.

```cpp
#include <bits/stdc++.h>
using namespace std;

int countSetBits(int n) {
    int count = 0;

    while (n > 0) {
        if ((n & 1) != 0) {
            count++;
        }

        n = n >> 1;
    }

    return count;
}

int main() {
    int n = 35;
    cout << countSetBits(n) << endl;

    return 0;
}
```

Time Complexity: `O(log N)`
Space Complexity: `O(1)`

## 4. Find the Least Significant Set Bit

Problem: Given a positive integer `N`, find the position of its rightmost set bit. Positions are 0-based.

Example:

```text
44 = 101100

The rightmost set bit is at position 2.
```

```cpp
#include <bits/stdc++.h>
using namespace std;

int leastSignificantSetBit(int num) {
    for (int i = 0; i < 32; i++) {
        if ((num & (1 << i)) != 0) {
            return i;
        }
    }

    return -1;
}

int main() {
    int n = 44;
    cout << leastSignificantSetBit(n) << endl;

    return 0;
}
```

## 5. Find Two Unique Elements

Problem: Given an array where every element appears exactly twice except for two unique elements, find the two unique elements.

Example:

```text
Input:  {3, 4, 5, 4, 7, 5}
Output: 3 7
```

```cpp
#include <bits/stdc++.h>
using namespace std;

int findSetBitPosition(int num) {
    for (int i = 0; i < 32; i++) {
        if ((num & (1 << i)) != 0) {
            return i;
        }
    }

    return -1;
}

bool checkBit(int num, int position) {
    return (num & (1 << position)) != 0;
}

int main() {
    vector<int> arr = {3, 4, 5, 4, 7, 5};

    int xorAll = 0;

    for (int value : arr) {
        xorAll ^= value;
    }

    int position = findSetBitPosition(xorAll);

    int firstUnique = 0;
    int secondUnique = 0;

    for (int value : arr) {
        if (checkBit(value, position)) {
            firstUnique ^= value;
        } else {
            secondUnique ^= value;
        }
    }

    cout << firstUnique << " " << secondUnique << endl;

    return 0;
}
```

How it works:

```text
1. XOR all array elements.
2. Duplicate elements cancel each other.
3. The result is XOR of the two unique numbers.
4. Find a bit where those two numbers differ.
5. Split the numbers into two groups using that bit.
6. XOR each group to get the two unique numbers.
```

Time Complexity: `O(N)`
Space Complexity: `O(1)`
