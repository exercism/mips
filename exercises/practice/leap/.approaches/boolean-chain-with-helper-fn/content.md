# Chain of boolean expressions with helper function

```asm
## Registers

# | Register | Usage     | Type    | Description                                      |
# | -------- | --------- | ------- | ------------------------------------------------ |
# | `$a0`    | input     | integer | year to check                                    |
# | `$v0`    | output    | boolean | input is leap year (`0` = `false`, `1` = `true`) |
# | `$t0-9`  | temporary | any     | used for temporary storage                       |

.globl is_leap_year

is_leap_year:
        addi    $sp, $sp, -4
        sw      $ra, 0($sp)
        li      $v0, 0

        li      $a1, 4
        jal     is_divisible_by
        beqz    $v1, end

        li      $a1, 100
        jal     is_divisible_by
        beqz    $v1, set_leap

        li      $a1, 400
        jal     is_divisible_by
        beqz    $v1, end

set_leap:
        li      $v0, 1
end:
        lw      $ra, 0($sp)
        addi    $sp, $sp, 4
        jr      $ra

is_divisible_by:
        div     $a0, $a1
        mfhi    $t0
        slti    $v1, $t0, 1
        jr      $ra
```

This approach performs three boolean tests in sequence, using the _remainder_ of a division to check if the year is evenly divisible by certain numbers,
just like the [traditional boolean chain][approach-boolean-chain] approach, but with a twist: it uses a helper function `is_divisible_by` to determine if the year is evenly divisible.
It calls the helper function with the `jal` (jump and link) instruction; but in order to do this, it needs to push the `$ra` register, which holds the return
address passed to the `is_leap_year` function, onto the stack to save it. Since there are no `push`/`pop` instructions on MIPS, this must be done by manually
manipulating the `$sp` (stack pointer) register.

Equivalent C code:

```c
int is_divisible_by(int year, int divisor) {
    return year % divisor == 0;
}

int is_leap_year(int year) {
    return is_divisible_by(year, 4) && !is_divisible_by(year, 100) || is_divisible_by(year, 400);
}
```

[approach-boolean-chain]: https://exercism.org/tracks/mips/exercises/leap/approaches/boolean-chain
