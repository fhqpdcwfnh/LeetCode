# Math problem

---
## 11. Set Mismatch

The set S originally contains numbers from 1 to n. But unfortunately, due to the data error, one of the numbers in the set got duplicated to another number in the set, which results in repetition of one number and loss of another number. Given an array nums representing the data status of this set after the error. Your task is to firstly find the number occurs twice and then find the number that is missing. Return them in the form of an array.

Example:

    Input: nums = [1,2,2,4]
    Output: [2,3]

Analysis: XOR data, for example: x ^ x = 0.

    for example: [1 2 3 2 5 6], and all numbers [1 2 3 4 5 6].
    In binary: [001 010 011 010 101 110], [001 010 011 100 101 110].

If we XOR the 2 arrays, we will get the XOR of our duplicate and missing number. In our case, missing ^ duplicate = 110. We know the missing and duplicate number are different in the first and second most significant bits (110). So let's take the last one: 110 -> get last 1 -> 010. Then, we go through the arrays once again and split them in 2 categories, if they have that bit set or not:

    (x & 010) != 0: [010 011 010 110], [010 011 110] -> XOR all of them and we get 010.
    (x & 010) == 0: [001 101], [001 100 101] -> XOR all of them and we get 100.

010 and 100 (2 and 4) are our duplicate and missing numbers, but we don't know which is which. Just go one more time through the array to see which one we can find, in our case: 010. So 2 is the duplicate, and 4 the missing one.

```c
/**
 * Return an array of size *returnSize.
 * Note: The returned array must be malloced, assume caller calls free().
 */
int* findErrorNums(int* nums, int numsSize, int* returnSize) {
    int i = 0;
    int res = 0;
    int res1 = 0;
    int res2 = 0;
    int *res3 = NULL;

    // xor all the data
    for (i = 0; i < numsSize; ++i)
    {
        res ^= (nums[i] ^ (i + 1));
    }

    // get the rightmost 1-bit
    res &= (-res);

    // find the x and y
    for (i = 0; i < numsSize; ++i)
    {
        ((nums[i] & res) != 0) ? (res1 ^= nums[i]) : (res2 ^= nums[i]);
        (((i + 1) & res) != 0) ? (res1 ^= (i + 1)) : (res2 ^= (i + 1));
    }

    // alloc memory
    *returnSize = 2;
    res3 = (int *)malloc(sizeof(int) * (*returnSize));
    res3[0] = res1;
    res3[1] = res2;

    for (i = 0; i < numsSize; ++i)
    {
        if (nums[i] == res2)
        {
            res3[0] = res2;
            res3[1] = res1;
        }
    }

    return res3;
}
```

---
## 10. Ugly Number

Write a program to check whether a given number is an ugly number. Ugly numbers are positive numbers whose prime factors only include 2, 3, 5. For example, 6, 8 are ugly while 14 is not ugly since it includes another prime factor 7.

Note:

    1 is typically treated as an ugly number.

```c
bool isUgly(int num) {
    uint i = 0;

    if (num <= 0)
    {
        return false;
    }

    for (i = 2; i < 6; ++i)
    {
        while (num % i == 0)
        {
            num /= i;
        }

        if (num == 1)
        {
            return true;
        }
    }

    return false;
}
```

---
## 9. Power of Two

Given an integer, write a function to determine if it is a power of two.

Analysis:

    Power of 2 means only one bit of n is ‘1’, so use n&(n-1)==0 to judge

```c
bool isPowerOfTwo(int n) {
    return !((n <= 0) || (n & (n - 1)));
}
```

---
## 8. Happy Number

Write an algorithm to determine if a number is "happy". A happy number is a number defined by the following process: Starting with any positive integer, replace the number by the sum of the squares of its digits, and repeat the process until the number equals 1 (where it will stay), or it loops endlessly in a cycle which does not include 1. Those numbers for which this process ends in 1 are happy numbers.

Example: 19 is a happy number

    12 + 92 = 82
    82 + 22 = 68
    62 + 82 = 100
    12 + 02 + 02 = 1

Analysis: [CSDN explaination](https://blog.csdn.net/Javasus/article/details/50015687); [CSDN explaination with code](https://blog.csdn.net/gdymind/article/details/70544153); [Wiki](https://en.wikipedia.org/wiki/Cycle_detection);

    Use Floyd Cycle Detection algorithm.

```c
int next(int n)
{
    int res=0;

    while (n != 0)
    {
        int t = n % 10;
        res += (t * t);
        n /= 10;
    }

    return res;
}

bool isHappy(int n) {
    int slow = n;
    int fast = n;

    do
    {
        slow = next(slow);
        fast = next(next(fast));

        if ((slow == 1) || (fast == 1))
        {
            return true;
        }
    }
    while (slow != fast);

    return false;
}
```

---
## 7. Hamming Distance

The Hamming distance between two integers is the number of positions at which the corresponding bits are different. Given two integers x and y, calculate the Hamming distance. Note: 0 ≤ x, y < 231.

Example:

    Input: x = 1, y = 4
    Output: 2

Explanation:

    1   (0 0 0 1)
    4   (0 1 0 0)
           ↑   ↑
The above arrows point to positions where the corresponding bits are different.

```c
int hammingDistance(int x, int y) {
    int count = 0;
    int t = x ^ y;

    while (t > 0)
    {
        t &= (t - 1);
        count++;
    }

    return count;
}
```

---
## 6. Maximum Product of Three Numbers

Given an integer array, find three numbers whose product is maximum and output the maximum product.

Example 1:

    Input: [1,2,3]
    Output: 6

Example 2:

    Input: [1,2,3,4]
    Output: 24

Note:

    The length of the given array will be in range [3,104] and all elements are in the range [-1000, 1000].
    Multiplication of any three numbers in the input won't exceed the range of 32-bit signed integer.

Analysis: qsort function, quick sort, parameter 1: array address, parameter 2: total number of array, parameter 3: element byte number, parameter 4: compare function

```c
int cmp(void *a, void *b)
{
    return *(int *)a - *(int *)b;
}

int maximumProduct(int* nums, int numsSize) {
    int ret1 = 0;
    int ret2 = 0;

    qsort(nums, numsSize, sizeof(int), cmp);

    ret1 = nums[numsSize - 1] * nums[numsSize - 2] * nums[numsSize - 3];
    ret2 = nums[0] * nums[1] * nums[numsSize - 1];
    
    return (ret1 >= ret2) ? ret1 : ret2;
}
```

---
## 5. Missing Number

Given an array containing n distinct numbers taken from 0, 1, 2, ..., n, find the one that is missing from the array.

Example 1:

    Input: [3,0,1]
    Output: 2

Example 2:

    Input: [9,6,4,2,3,5,7,0,1]
    Output: 8

```c
int missingNumber(int* nums, int numsSize) {
    int i = 0;
    int sum = 0;
    
    for (i = 0; i < numsSize; ++i)
    {
        sum += nums[i];
    }
    
    return ((numsSize + 1) * numsSize / 2 - sum);
}
```

---
## 4. Minimum Moves to Equal Array Elements

Given a non-empty integer array of size n, find the minimum number of moves required to make all array elements equal, where a move is incrementing n - 1 elements by 1.

Example:

    Input: [1,2,3]
    Output: 3

Explanation:

    Only three moves are needed (remember each move increments two elements):
    [1,2,3]  =>  [2,3,3]  =>  [3,4,3]  =>  [4,4,4]

```c
int minMoves(int* nums, int numsSize) {
    int i = 0;
    int sum = nums[0];
    int min = nums[0];

    for (i = 1; i < numsSize; ++i)
    {
        min = (min < nums[i]) ? min : nums[i];
        sum += nums[i];
    }
    
    return (sum - (min * numsSize));
}
```

---
## 3. Excel Sheet Column Number

Given a column title as appear in an Excel sheet, return its corresponding column number.

Example:

    A -> 1
    B -> 2
    C -> 3
    ...
    Z -> 26
    AA -> 27
    AB -> 28 

```c
int titleToNumber(char* s) {
    int i = 0;
    int num = 0;
 
    for (i = 0; s[i] != '\0'; ++i)
    {
        num = num * 26 + s[i] - 'A' + 1;
    }

    return num;
}
```

---
## 2. Add Digits

Given a non-negative integer num, repeatedly add all its digits until the result has only one digit. For example: Given num = 38, the process is like: 3 + 8 = 11, 1 + 1 = 2. Since 2 has only one digit, return it.

```c
int addDigits(int num) {
    int t1 = num;
    int t2 = num;
    int sum = 0;

    while (t1 > 9)
    {
        sum = 0;
        t2 = t1;

        while (t2 != 0)
        {
            sum += (t2 % 10);
            t2 /= 10;
        }

        t1 = sum;
    }

    return t1;
}
```

Analisys:

    follow below rule, and find number mod 9, and the resule very close to expected result.

    0->0
    1->1
    ...
    9->9
    10->1
    11->2
    ...
    18->9
    19->1
    ...

```c
int addDigits(int num) {
    int t = num;

    if (t == 0)
    {
        return 0;
    }
    else
    {
        t %= 9;
        return ((t == 0) ? 9 : t);
    }
}
```

---
## 1. Self Dividing Numbers

A self-dividing number is a number that is divisible by every digit it contains. For example, 128 is a self-dividing number because 128 % 1 == 0, 128 % 2 == 0, and 128 % 8 == 0. Also, a self-dividing number is not allowed to contain the digit zero. Given a lower and upper number bound, output a list of every possible self dividing number, including the bounds if possible.

Example:

    Input: left = 1, right = 22
    Output: [1, 2, 3, 4, 5, 6, 7, 8, 9, 11, 12, 15, 22]

Note:

    The boundaries of each input argument are 1 <= left <= right <= 10000.

```c
/**
 * Return an array of size *returnSize.
 * Note: The returned array must be malloced, assume caller calls free().
 */
bool isSelfDividingNumber(int num)
{
    int temp = num;

    while (temp != 0)
    {
        if (((temp % 10) == 0) || ((num % (temp % 10)) != 0))
        {
            return false;
        }
        else
        {
            temp /= 10;
        }
    }

    return true;
}

int* selfDividingNumbers(int left, int right, int* returnSize) {
    int i = 0;
    int *array = NULL;
    *returnSize = 0;

    for (i = left; i <= right; ++i)
    {
        if (isSelfDividingNumber(i))
        {
            (*returnSize)++;
        }
    }

    array = (int *)malloc(sizeof(int) * (*returnSize));

    *returnSize = 0;

    for (i = left; i <= right; ++i)
    {
        if (isSelfDividingNumber(i))
        {
            array[(*returnSize)++] = i;
        }
    }

    return array;
}
```
