# Using the `andi` instruction

```asm
## Registers

# | Register | Usage     | Type    | Description                                      |
# | -------- | --------- | ------- | ------------------------------------------------ |
# | `$a0`    | input     | integer | year to check                                    |
# | `$v0`    | output    | boolean | input is leap year (`0` = `false`, `1` = `true`) |
# | `$t0-9`  | temporary | any     | used for temporary storage                       |

.globl is_leap_year

is_leap_year:
        li      $v0, 0

        andi    $t0, $a0, 3
        bnez    $t0, end

        li      $t0, 100
        div     $a0, $t0
        mfhi    $t0
        bnez    $t0, set_leap

        mflo    $t0
        andi    $t0, $t0, 3
        bnez    $t0, end

set_leap:
        li      $v0, 1
end:
        jr      $ra
```

Equivalent C code:

```c
#include <stdlib.h>

int is_leap_year(int year) {
    if ((year & 3) != 0) {
        return 0;
    }

    div_t by_100 = div(year, 100);
    return by_100.rem != 0 || (by_100.quot & 3) == 0;
}
```

This approach uses the `andi` instruction to determine if the year is divisible by `4`
by relying on the fact that if a number is divisible by `4`, its two least significant bits
will be zero.

To determine if the year is divisible by `100`, we then use `div` combined with `mfhi` to get
the remainder. Finally, if we need to check if the year is divisible by `400`, we use another
trick: calling `div` left the quotient of the division by `100` in the `lo` register. Thus,
we can fetch that quotient using `mflo` and then re-use the same trick as before to determine
if that quotient is divisible by `4` (making the year divisible by `400`).
