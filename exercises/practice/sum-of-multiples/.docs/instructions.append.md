# Instructions append

## Registers

| Register | Usage     | Type    | Description                   |
| -------- | --------- | ------- | ----------------------------- |
| `$a0`    | input     | address | array of factor words         |
| `$a1`    | input     | integer | number of factors             |
| `$a2`    | input     | integer | limit                         |
| `$v0`    | output    | integer | sum of multiples within limit |
| `$t0-9`  | temporary | any     | used for temporary storage    |
