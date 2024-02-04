# Instructions append

The output should be arranged in [row-major order](https://en.wikipedia.org/wiki/Row-_and_column-major_order): all the words in the first row, then the words in the second row, and so on.

## Registers

| Register | Usage        | Type    | Description                         |
| -------- | ------------ | ------- | ----------------------------------- |
| `$a0`    | input        | integer | size                                |
| `$a1`    | input/output | address | matrix of words, in row-major order |
| `$v0`    | output       | integer | number of words in matrix           |
| `$t0-9`  | temporary    | any     | for temporary storage               |
