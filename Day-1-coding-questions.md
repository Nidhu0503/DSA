# Basic Problem Solving

## 1. Check Whether a Number Is Prime

A prime number has exactly two factors: `1` and itself.

Examples:

```text
2, 3, 5, 7, 11 are prime numbers.
0 and 1 are not prime numbers.
```

### Approach 1: Count Factors

```cpp
#include <bits/stdc++.h>
using namespace std;

bool isPrime(int n) {
    if (n <= 1) {
        return false;
    }

    int count = 0;

    for (int i = 1; i <= n; i++) {
        if (n % i == 0) {
            count++;
        }
    }

    return count == 2;
}

int main() {
    int n;
    cin >> n;

    if (isPrime(n)) {
        cout << n << " is a prime number" << endl;
    } else {
        cout << n << " is not a prime number" << endl;
    }

    return 0;
}
```

Time Complexity: `O(N)`

### Approach 2: Check Until Square Root

If `n` has a factor greater than `√n`, it must also have a factor smaller than `√n`.

```cpp
#include <bits/stdc++.h>
using namespace std;

bool isPrime(int n) {
    if (n <= 1) {
        return false;
    }

    for (int i = 2; i * i <= n; i++) {
        if (n % i == 0) {
            return false;
        }
    }

    return true;
}

int main() {
    int n;
    cin >> n;

    if (isPrime(n)) {
        cout << n << " is a prime number" << endl;
    } else {
        cout << n << " is not a prime number" << endl;
    }

    return 0;
}
```

Time Complexity: `O(√N)`

---

## 2. Sqrt(x)

Problem link: [LeetCode - Sqrt(x)](https://leetcode.com/problems/sqrtx/description/)

Given a non-negative integer `x`, return the integer square root of `x`.

Example:

```text
x = 8
Square root = 2
```

Because:

```text
2 × 2 = 4
3 × 3 = 9

The integer square root of 8 is 2.
```

Practice both approaches:

```text
1. Brute force
2. Binary search
```

---

## 3. Valid Perfect Square

Problem link: [LeetCode - Valid Perfect Square](https://leetcode.com/problems/valid-perfect-square/description/)

Given a positive integer `num`, return true if it is a perfect square.

Examples:

```text
16 → true
14 → false
```

Practice both approaches:

```text
1. Brute force
2. Binary search
```

---

## 4. Smaller and Greater

Problem: Given an array `A`, count how many elements have at least one strictly smaller element and at least one strictly greater element.

Example:

```text
Input:  [1, 2, 3]
Output: 1
```

Only `2` has both a smaller element (`1`) and a greater element (`3`).

```cpp
int Solution::solve(vector<int>& A) {
    sort(A.begin(), A.end());

    int n = A.size();
    int minimum = A[0];
    int maximum = A[n - 1];
    int count = 0;

    for (int i = 0; i < n; i++) {
        if (A[i] > minimum && A[i] < maximum) {
            count++;
        }
    }

    return count;
}
```

Time Complexity: `O(N log N)`
Space Complexity: `O(1)` excluding the sorting implementation.

---

## 5. Elements With At Least Two Greater Elements

Problem: Given an array of distinct integers, return every element that has at least two greater elements in the array.

The original order must be preserved.

Example:

```text
Input:  [1, 2, 3, 4, 5]
Output: [1, 2, 3]
```

Example:

```text
Input:  [11, 17, 100, 5]
Output: [11, 5]
```

Approach:

```text
1. Find the largest and second-largest elements.
2. Every element smaller than the second-largest element has at least two greater elements.
3. Traverse the original array again and collect those elements.
```

```cpp
vector<int> Solution::solve(vector<int>& A) {
    int largest = INT_MIN;
    int secondLargest = INT_MIN;

    for (int value : A) {
        if (value > largest) {
            secondLargest = largest;
            largest = value;
        } else if (value > secondLargest) {
            secondLargest = value;
        }
    }

    vector<int> answer;

    for (int value : A) {
        if (value < secondLargest) {
            answer.push_back(value);
        }
    }

    return answer;
}
```

Time Complexity: `O(N)`
Space Complexity: `O(N)` for the output array.

---

## 6. Minimum Picks

Problem: Return:

```text
Maximum even number - Minimum odd number
```

There is always at least one even number and one odd number.

Example:

```text
Input:  [0, 2, 9]
Output: -7

Maximum even number = 2
Minimum odd number = 9

2 - 9 = -7
```

```cpp
int Solution::solve(vector<int>& A) {
    int maxEven = INT_MIN;
    int minOdd = INT_MAX;

    for (int value : A) {
        if (value % 2 == 0) {
            maxEven = max(maxEven, value);
        } else {
            minOdd = min(minOdd, value);
        }
    }

    return maxEven - minOdd;
}
```

Time Complexity: `O(N)`
Space Complexity: `O(1)`

---

## 7. Pattern Printing 1

Problem: Given `A`, return a two-dimensional array with the following pattern.

Example for `A = 3`:

```text
1 0 0
1 2 0
1 2 3
```

Example for `A = 4`:

```text
1 0 0 0
1 2 0 0
1 2 3 0
1 2 3 4
```

```cpp
vector<vector<int>> Solution::solve(int A) {
    vector<vector<int>> answer(A, vector<int>(A, 0));

    for (int i = 0; i < A; i++) {
        for (int j = 0; j <= i; j++) {
            answer[i][j] = j + 1;
        }
    }

    return answer;
}
```

Time Complexity: `O(A²)`
Space Complexity: `O(A²)`
