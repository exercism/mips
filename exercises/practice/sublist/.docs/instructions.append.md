# Instructions append

## Registers

| Register | Usage     | Type    | Description                                                |
| -------- | --------- | ------- | ---------------------------------------------------------- |
| `$a0`    | input     | address | array one elements                                         |
| `$a1`    | input     | integer | size of array one, in words                                |
| `$a2`    | input     | address | array two elements                                         |
| `$a3`    | input     | integer | size of array two, in words                                |
| `$v0`    | output    | integer | `0` = equal, `1` = unequal, `2` = sublist, `3` = superlist |
| `$t0-9`  | temporary | any     | used for temporary storage                                 |
