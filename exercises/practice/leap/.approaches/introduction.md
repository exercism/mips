# Introduction

Although MIPS assembly is more limited because it does not provide access to actual date/time functions,
there are still multiple approaches to solve Leap. You can use a chain of boolean operators to test
the conditions, in traditional or reverse order. You can also check all conditions and then use bitwise
operators to compute the result. It is also possible to use a helper function to check if the year
is evenly divisible by a certain number.

## General guidance

The key to solving Leap is to know if the year is evenly divisible by `4`, `100` and `400`.
For determining that, we need to use the _remainder_ of the division. MIPS does not have a remainder
instruction, however: instead, the `div` instruction computes _both_ the quotient and the remainder
of the division and stores them in the `lo` and `hi` registers, respectively.

Also, the `lo` and `hi` registers cannot be accessed directly by most instructions. To get their
values, we need to _move_ them to another register with the help of instructions like `mfhi` (move from high).

For a reference of MIPS instructions, check out the [MIPS green sheet][mips-green-sheet].

## Approach: Chain of Boolean expressions

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

For more information, check the [Boolean chain approach][approach-boolean-chain].

## Approach: Reverse chain of boolean expressions

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

For more information, check the [Reverse boolean chain approach][approach-boolean-chain-reverse].

## Approach: Chain of boolean expressions with helper function

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

For more information, check the [Boolean chain with helper function approach][approach-boolean-chain-with-helper-fn].

## Approach: Bitwise operations

```asm
## Registers

# | Register | Usage     | Type    | Description                                      |
# | -------- | --------- | ------- | ------------------------------------------------ |
# | `$a0`    | input     | integer | year to check                                    |
# | `$v0`    | output    | boolean | input is leap year (`0` = `false`, `1` = `true`) |
# | `$t0-9`  | temporary | any     | used for temporary storage                       |

.globl is_leap_year

is_leap_year:
        li      $t0, 4
        div     $a0, $t0
        mfhi    $t1
        slti    $t1, $t1, 1

        li      $t0, 100
        div     $a0, $t0
        mfhi    $t2
        li      $t7, 0
        slt     $t2, $t7, $t2

        li      $t0, 400
        div     $a0, $t0
        mfhi    $t3
        slti    $t3, $t3, 1

        and     $v0, $t1, $t2
        or      $v0, $v0, $t3
        beqz    $v0, end

set_leap:
        li      $v0, 1
end:
        jr      $ra
```

For more information, check the [Bitwise operations approach][approach-bitwise].

## Which approach to use?

- The chain of boolean expressions is most efficient, as it proceeds from the most likely to least likely conditions.
  It has a maximum of three checks.
- The reverse chain of boolean expressions also has a maximum of three checks, but proceeds from the least likely conditions
  and can be considered less efficient.
- Using a helper function with a chain of boolean expressions is similar but could perhaps make the code more readable, at
  the cost of additional jumps when executing it.
- The bitwise operations approach always needs to perform the three checks, so it can be considered less efficient.

[mips-green-sheet]: https://inst.eecs.berkeley.edu/~cs61c/resources/MIPS_Green_Sheet.pdf
[approach-boolean-chain]: https://exercism.org/tracks/mips/exercises/leap/approaches/boolean-chain
[approach-boolean-chain-reverse]: https://exercism.org/tracks/mips/exercises/leap/approaches/boolean-chain-reverse
[approach-boolean-chain-with-helper-fn]: https://exercism.org/tracks/mips/exercises/leap/approaches/boolean-chain-with-helper-fn
[approach-bitwise]: https://exercism.org/tracks/mips/exercises/leap/approaches/bitwise
