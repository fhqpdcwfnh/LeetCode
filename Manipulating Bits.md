|               |         1 -> 0         |         0 -> 1         |
|:-------------:|:----------------------:|:----------------------:|
| rightmost bit |      x & (x - 1)       |      x \| (x + 1)      |
|  for example  |  01011000 -> 01010000  |  10100111 -> 10101111  |
| tailing bits  |      x & (x + 1)       |      x \| (x - 1)      |
|  for example  |  10100111 -> 10100000  |  10101000 -> 10101111  |
|               | other bits not changed | other bits not changed |

|               |          0 -> 1         |         0 -> 1          |
|:-------------:|:-----------------------:|:-----------------------:|
| rightmost bit |      ~x & (x - 1)       |      ~x \| (x + 1)      |
|  for example  |  01011000 -> 00000111   |  10100111 -> 10101111   |
| tailing bits  |      ~x & (x + 1)       |      ~x \| (x - 1)      |
|  for example  |  01011000 -> 01010000   |  10100111 -> 10101111   |
|               | other bits changed to 0 | other bits changed to 1 |
