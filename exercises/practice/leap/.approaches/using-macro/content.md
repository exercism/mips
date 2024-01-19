# Using a "divisible by" macro

```asm
## Registers

# | Register | Usage     | Type    | Description                                      |
# | -------- | --------- | ------- | ------------------------------------------------ |
# | `$a0`    | input     | integer | year to check                                    |
# | `$v0`    | output    | boolean | input is leap year (`0` = `false`, `1` = `true`) |
# | `$t0-9`  | temporary | any     | used for temporary storage                       |

.globl is_leap_year

.macro if_divisible_by($divisor, $then_jump)
        li      $t0, $divisor
        div     $a0, $t0
        mfhi    $t0
        beqz    $t0, $then_jump
.end_macro

is_leap_year:
        if_divisible_by(400, set_leap)
        if_divisible_by(100, not_leap)
        if_divisible_by(4, set_leap)

not_leap:
        li      $v0, 0
        jr      $ra

set_leap:
        li      $v0, 1
        jr      $ra
```

Equivalent C code:

```c
#define IF_DIVISIBLE_BY_THEN_RETURN(year, divisor, ret) \
    if (year % divisor == 0) {  \
        return ret;             \
    }
    
int is_leap_year(int year) {
    IF_DIVISIBLE_BY_THEN_RETURN(year, 400, 1);
    IF_DIVISIBLE_BY_THEN_RETURN(year, 100, 0);
    IF_DIVISIBLE_BY_THEN_RETURN(year, 4, 1);
    return 0;
}
```

This approach uses the `div` and `mfhi` instructions to check if the year
is divisible by a value, but defines a macro to avoid repeating the code
that checks the remainder of the division.
