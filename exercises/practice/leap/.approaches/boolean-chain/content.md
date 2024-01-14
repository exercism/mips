# Chain of boolean expressions

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

This approach performs three boolean tests in sequence, using the _remainder_ of a division to check if the year is evenly divisible by certain numbers.

- First, it checks if the year is evenly divisible by `4`.
  - If it is not, it returns `0` (not leap) immediately, "short circuiting" the chain.
  - Otherwise, we move on to the next test.
- Then, it checks if the year is evenly divisible by `100`.
  - If it is not, it returns `1` (leap) immediately.
  - Otherwise, we move on to the next test.
- Finally, it checks if the year is evenly divisible by `400`.
  - If it is not, it returns `0` (not leap).
  - Otherwise, it returns `1` (leap).

| year | year % 4 == 0 | year % 100 != 0 | year % 400 == 0 | is leap year |
| ---- | ------------- | --------------- | --------------- | ------------ |
| 2020 | true          | true            | not evaluated   | true         |
| 2019 | false         | not evaluated   | not evaluated   | false        |
| 2000 | true          | false           | true            | true         |
| 1900 | true          | false           | false           | false        |

Equivalent C code:

```c
int is_leap_year(int year) {
    return year % 4 == 0 && year % 100 != 0 || year % 400 == 0;
}
```

The chain of boolean expressions is efficient, as it proceeds from testing the most likely to least likely conditions.
[Howard Hinnant][hinnant] wrote a short paragraph about the order of operations on his web page about date-related algorithms.

[hinnant]: https://howardhinnant.github.io/date_algorithms.html#is_leap
