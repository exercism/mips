# Instructions append

## Registers

| Register | Usage     | Type    | Description                                                |
| -------- | --------- | ------- | ---------------------------------------------------------- |
| `$a0`    | input     | integer | category: `0` = choice, `1` = ones, `2` = twos, `3` = threes, `4` = fours, `5` = fives, `6` = sixes, `7` = little_straight, `8` = big_straight, `9` = full_house, `10` = four_of_a_kind, `11` = yacht |
| `$a2`    | input     | address | dice (word array)                                          |
| `$v0`    | output    | integer | resulting score                                            |
| `$t0-9`  | temporary | any     | used for temporary storage                                 |
