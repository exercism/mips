# Instructions append

## Registers

| Register | Usage     | Type    | Description                                       |
| -------- | --------- | ------- | ------------------------------------------------- |
| `$a0`    | input     | address | null-terminated input string                      |
| `$v0`    | output    | boolean | input is an pangram (`0` = `false`, `1` = `true`) |
| `$t0-9`  | temporary | any     | used for temporary storage                        |
