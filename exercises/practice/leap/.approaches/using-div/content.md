# Using the `div` instruction

```asm
## Registers

# | Register | Usage     | Type    | Description                                           |
# | -------- | --------- | ------- | ----------------------------------------------------- |
# | `$a0`    | input     | integer | year to check                                         |
# | `$v0`    | output    | boolean | input is leap year (`0` = `false`, `1` = `true`)      |
# | `$t0`    | temporary | boolean | year is multiple of 4 (`0` = `false`, `1` = `true`)   |
# | `$t1`    | temporary | boolean | year is multiple of 100 (`0` = `false`, `1` = `true`) |
# | `$t2`    | temporary | boolean | year is multiple of 400 (`0` = `false`, `1` = `true`) |

.globl is_leap_year

is_leap_year:
        li      $t0, 4
        div     $a0, $t0
        mfhi    $t0
        seq     $t0, $t0, $zero

        li      $t1, 100
        div     $a0, $t1
        mfhi    $t1
        seq     $t1, $t1, $zero

        li      $t2, 400
        div     $a0, $t2
        mfhi    $t2
        seq     $t2, $t2, $zero

        sub     $v0, $t0, $t1
        add     $v0, $v0, $t2
        jr      $ra
```

Equivalent C code:

```c
int is_leap_year(int year) {
    int is_multiple_4 = (year % 4 == 0);
    int is_multiple_100 = (year % 100 == 0);
    int is_multiple_400 = (year % 400 == 0);
    return is_multiple_4 - is_multiple_100 + is_multiple_400;
}
```

This approach uses the `div` instruction to compute the remainder of divisions.
This instruction actually computes both the quotient _and_ the remainder of the division and
places them in the `lo` and `hi` registers, respectively.

However, those registers cannot be used as arguments to most instructions. Instead, in order
to use their values, we need to _move_ them from the `lo` or `hi` registers into another
register using the `mfhi` (move from hi) or `mflo` (move from lo) instructions.

The `seq` instruction sets a register to 1 if the two values are equal, and otherwise sets the register to 0.
