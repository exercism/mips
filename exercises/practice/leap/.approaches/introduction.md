# Introduction

Although MIPS assembly is more limited because it does not provide access to actual date/time functions,
there are still multiple approaches to solve Leap that involve different MIPS instructions.

## General guidance

The key to solving Leap is to know if the year is evenly divisible by `4`, `100` and `400`.
There are several instructions that can be used to help us determine this, like `div` (divide),
`rem` (modulo) and `andi` (bitwise AND immediate). Once a check has been made, we can use
instructions like `bnez` (branch if not equal zero) to short circuit the other checks:

| year | year divisible by 4 | year divisible by 100 | year divisible by 400 | is leap year |
| ---- | ------------------- | --------------------- | --------------------- | ------------ |
| 2020 | true                | false                 | not evaluated         | true         |
| 2019 | false               | not evaluated         | not evaluated         | false        |
| 2000 | true                | true                  | true                  | true         |
| 1900 | true                | true                  | false                 | false        |

For more information on the MIPS instructions used, you can refer to this [MIPS instructions reference][mips-instructions-ref]
or to this [MIPS green sheet][mips-green-sheet].

## Approach: Using the `div` instruction

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

For more information, check the [approach using `div`][approach-using-div].

## Approach: Using the `rem` instruction

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

For more information, check the [approach using `rem`][approach-using-rem].

## Approach: Using a "divisible by" macro

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

For more information, check the [approach using a macro][approach-using-macro].

## Approach: Using the `andi` instruction

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

For more information, check the [approach using `andi`][approach-using-andi].
 

[mips-instructions-ref]: https://pages.cs.wisc.edu/~markhill/cs354/Fall2008/notes/MAL.instructions.html
[mips-green-sheet]: https://inst.eecs.berkeley.edu/~cs61c/resources/MIPS_Green_Sheet.pdf
[approach-using-div]: https://exercism.org/tracks/mips/exercises/leap/approaches/using-div
[approach-using-rem]: https://exercism.org/tracks/mips/exercises/leap/approaches/using-rem
[approach-using-macro]: https://exercism.org/tracks/mips/exercises/leap/approaches/using-macro
[approach-using-andi]: https://exercism.org/tracks/mips/exercises/leap/approaches/using-andi
