# Math problem

## 3. Excel Sheet Column Number

Given a column title as appear in an Excel sheet, return its corresponding column number.

For example:

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

Analisys: follow below rule, and find number mod 9, and the resule very close to expected result.

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
