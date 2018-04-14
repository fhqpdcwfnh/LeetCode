# Math problem
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

