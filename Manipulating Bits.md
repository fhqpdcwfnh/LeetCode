|               |         1 -> 0         |         0 -> 1         |
|:-------------:|:----------------------:|:----------------------:|
| rightmost bit |      x & (x - 1)       |      x \| (x + 1)      |
| for example   |  01011000 -> 01010000  |  10100111 -> 10101111  |
| tailing bits  |      x & (x + 1)       |      x \| (x - 1)      |
| for example   |  10100111 -> 10100000  |  10101000 -> 10101111  |
| note          | other bits not changed | other bits not changed |

|               |          0 -> 1         |         0 -> 1          |
|:-------------:|:-----------------------:|:-----------------------:|
| rightmost bit |      ~x & (x + 1)       |      ~x \| (x - 1)      |
| for example   |  10100111 -> 00001000   |  10101000 -> 11110111   |
| tailing bits  |      ~x & (x - 1)       |      ~x \| (x + 1)      |
| for example   |  01011000 -> 00000111   |  10100111 -> 11111000   |
| note          | other bits changed to 0 | other bits changed to 1 |

|   formula   |                                     explanation                                    |     for example      |
|:-----------:|:----------------------------------------------------------------------------------:|:--------------------:|
|  x & (-x)   | get the rightmost 1-bit, set other bits 0                                          | 01011000 -> 00001000 |
| x ^ (x - 1) | get the rightmost 1-bit, set 1 at the position of tailing 0-bits, set other bits 0 | 01011000 -> 00001111 |
| x ^ (x + 1) | set 1 at the position of rightmost 0-bit, get tailing 1-bits, set other bits 0     | 01010111 -> 00001111 |
