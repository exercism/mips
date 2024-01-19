# Using the `rem` instruction

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

        rem     $t0, $a0, 4
        bnez    $t0, end

        rem     $t0, $a0, 100
        bnez    $t0, set_leap

        rem     $t0, $a0, 400
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

This approach uses the `rem` instruction to compute the remainder of divisions.
Since this instruction computes the remainder and stores it in a register directly,
it is more concise than using `div`.
