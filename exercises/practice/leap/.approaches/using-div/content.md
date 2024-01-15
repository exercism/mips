# Using the `div` instruction

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

        li      $t0, 4
        div     $a0, $t0
        mfhi    $t0
        bnez    $t0, end

        li      $t0, 100
        div     $a0, $t0
        mfhi    $t0
        bnez    $t0, set_leap

        li      $t0, 400
        div     $a0, $t0
        mfhi    $t0
        bnez    $t0, end

set_leap:
        li      $v0, 1
end:
        jr      $ra
```

Equivalent C code:

```c
int is_leap_year(int year) {
    return year % 4 == 0 && (year % 100 != 0 || year % 400 == 0);
}
```

This approach uses the `div` instruction to compute the remainder of divisions.
This instruction actually computes both the quotient _and_ the remainder of the division and
places them in the `lo` and `hi` registers, respectively.

However, those registers cannot be used as arguments to most instructions. Instead, in order
to use their values, we need to _move_ them from the `lo` or `hi` registers into another
register using the `mfhi` (move from hi) or `mflo` (move from lo) instructions.
