|               |    1 -> 0   |    0 -> 1    |
|:-------------:|:-----------:|:------------:|
| rightmost bit | x & (x - 1) | x \| (x + 1) |
| tailing bits  | x & (x + 1) | x \| (x - 1) |
|               | other bits not changed | other bits not changed |
