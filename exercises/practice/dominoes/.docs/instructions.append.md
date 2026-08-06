# Instructions append

## Input format

Each domino is stored as a single byte, with the left half in the
high [nibble] and the right half in the low [nibble]. For example,
stones `[2|1]`, `[2|3]`, and `[1|3]` are represented as the bytes
`0x21 0x23 0x13`.

## Registers

| Register | Usage     | Type    | Description                                      |
| -------- | --------- | ------- | ------------------------------------------------ |
| `$a0`    | input     | address | array of stone bytes                             |
| `$a1`    | input     | integer | number of stones                                 |
| `$v0`    | output    | boolean | dominoes can chain (`0` = `false`, `1` = `true`) |
| `$t0-9`  | temporary | any     | used for temporary storage                       |


[nibble]: https://en.wikipedia.org/wiki/Nibble
