# Instructions append

## Registers

| Register | Usage     | Type    | Description                                      |
| -------- | --------- | ------- | ------------------------------------------------ |
| `$a0`    | input     | integer | year to check                                    |
| `$v0`    | output    | boolean | input is leap year (`0` = `false`, `1` = `true`) |
| `$t0-9`  | temporary | any     | used for temporary storage                       |
