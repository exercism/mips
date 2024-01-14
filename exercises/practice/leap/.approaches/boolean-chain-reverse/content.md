# Reverse chain of boolean expressions

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

        li      $t0, 400
        div     $a0, $t0
        mfhi    $t0
        beqz    $t0, set_leap

        li      $t0, 100
        div     $a0, $t0
        mfhi    $t0
        beqz    $t0, end

        li      $t0, 4
        div     $a0, $t0
        mfhi    $t0
        bnez    $t0, end

set_leap:
        li      $v0, 1
end:
        jr      $ra
```

This approach performs three boolean tests in sequence, using the _remainder_ of a division to check if the year is evenly divisible by certain numbers.
Contrarily to the [traditional boolean chain][approach-boolean-chain] approach however, it tests the outlier conditions first.

- First, it checks if the year is evenly divisible by `400`.
  - If it is, it returns `1` (leap) immediately, "short circuiting" the chain.
  - Otherwise, we move on to the next test.
- Then, it checks if the year is evenly divisible by `100`.
  - If it is, it returns `0` (not leap) immediately.
  - Otherwise, we move on to the next test.
- Finally, it checks if the year is evenly divisible by `4`.
  - If it is not, it returns `0` (not leap).
  - Otherwise, it returns `1` (leap).

| year | year % 400 == 0 | year % 100 != 0 | year % 4 == 0 | is leap year |
| ---- | --------------- | --------------- | ------------- | ------------ |
| 2020 | false           | true            | true          | true         |
| 2019 | false           | true            | false         | false        |
| 2000 | true            | not evaluated   | not evaluated | true         |
| 1900 | false           | false           | not evaluated | false        |

Equivalent C code:

```c
int is_leap_year(int year) {
    return year % 400 == 0 || year % 100 != 0 && year % 4 == 0;
}
```

Because this approach tests the outlier conditions first, it is more likely to execute more instructions
in order to determine if the year is a leap year and can be considered less efficient.

[approach-boolean-chain]: https://exercism.org/tracks/mips/exercises/leap/approaches/boolean-chain
