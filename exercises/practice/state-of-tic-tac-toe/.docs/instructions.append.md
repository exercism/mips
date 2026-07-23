# Instructions append

## Board format

The board is represented as a null-terminated string, with a newline character at the end of each row.

An example would be `"X  \n XO\nOX \n"`

## Registers

| Register | Usage        | Type    | Description                                                             |
| -------- | ------------ | ------- | ----------------------------------------------------------------------- |
| `$a0`    | input        | address | null-terminated input string                                            |
| `$v0`    | output       | integer | game state (`1` = `ongoing`, `2` = `draw`, `3` = `win`, `-1` = `error`) |
| `$t0-9`  | temporary    | any     | for temporary storage                                                   |
